# Plan for issue #68

## Branch

`fix/68-empty-index`

## Diagnosis

The failure happens when `KeywordSearcher.index()` receives an empty list.

From my Unit 2 reproduction, the existing test reaches:

`searcher.index([])`

and `KeywordSearcher.index()` builds an empty `tokenized_corpus` and passes it to:

`BM25Okapi(tokenized_corpus)`

The traceback then reaches:

`self.avgdl = num_doc / self.corpus_size`

inside `rank_bm25`, where the corpus size is zero, causing:

`ZeroDivisionError: division by zero`

The existing `search()` method already returns an empty list when there is no BM25 index or no chunks, so the failure occurs earlier during index creation.

## Scope

I will:
- handle an empty chunk list before constructing `BM25Okapi`
- make sure an empty index leaves the searcher in an empty state
- remove the existing `xfail` marker from the empty-index unit test once the behavior passes
- rerun the focused `KeywordSearcher` tests

I will also add coverage for stale state: index a non-empty corpus first, then call `index([])`, and verify that `searcher.bm25 is None`. I will also confirm that a later search returns `[]`. Checking `bm25` directly proves that the previous BM25 model was actually cleared rather than merely hidden by the empty `chunks` guard in `search()`.

I will not:
- change BM25 scoring for non-empty indexes
- modify the `rank-bm25` dependency
- refactor unrelated retrieval code
- change other search or ranking behavior

## Files to touch

### `rag/retriever/keyword_search.py`

Add an empty-input guard in `KeywordSearcher.index()` before `BM25Okapi` is constructed.

If the provided chunk list is empty, the method should leave `self.bm25` as `None` and return without creating a BM25 index.

This also prevents stale state if `index([])` is called after a previous non-empty index.

### `tests/unit/test_keyword_search.py`

Remove the existing `xfail` marker from `test_empty_index` so it becomes a normal regression test.

The current assertion already captures the expected behavior:

`assert results == []`

## Implementation approach

1. Keep `self.chunks = chunks` as the current indexed chunk state.
2. Check whether `chunks` is empty before constructing the BM25 model.
3. If empty, set `self.bm25 = None` and return.
4. Leave the existing BM25 construction path unchanged for non-empty chunks.
5. Remove the `xfail` marker from `test_empty_index`.
6. Run the focused empty-index test and then the full keyword-search unit test file.

## Test plan

Before the fix, I reproduced the issue with:

`pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --runxfail`

The observed result was:

`ZeroDivisionError: division by zero`

After the fix I will run:

`pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv`

Expected result:

The test should pass instead of raising `ZeroDivisionError`, and searching after `index([])` should return `[]`.

I will then run:

`pytest tests/unit/test_keyword_search.py -vv`

Expected result:

The full keyword-search unit test file should pass without changing behavior for non-empty indexes.

I will also run a regression test that indexes a non-empty corpus, re-indexes with `[]`, and confirms that searching returns `[]`. This verifies that the fix clears any previous BM25 state instead of only avoiding the constructor error.

Finally, I will run the repository checks required by the contribution guide:

`make check`

`make test-unit`

## Risks and unknowns

One risk is stale state if the same `KeywordSearcher` first indexes a non-empty corpus and later receives `index([])`. The implementation should clear `self.bm25` so a later search cannot use the old BM25 model.

I do not expect a change to `rank-bm25` itself to be necessary because the application can avoid constructing it for an empty corpus.

## Deviations

The implementation matched the posted plan. I added the empty-index guard in `KeywordSearcher.index()`, cleared any previous BM25 state when re-indexing with an empty list, removed the existing `xfail` marker, and added the planned stale-state regression test. No additional files or scope changes were needed.