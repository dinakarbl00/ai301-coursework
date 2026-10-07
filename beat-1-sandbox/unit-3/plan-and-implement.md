# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

dinakarbl00

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-6034754279

I reproduced the empty-index `ZeroDivisionError` from #68 with:

`pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --runxfail`

The failure occurs when `KeywordSearcher.index()` passes an empty corpus into `BM25Okapi`, which raises `ZeroDivisionError` during initialization.

My plan is to add an empty-input guard before the BM25 model is constructed, make sure an empty re-index clears any previous BM25 state, and remove the existing `xfail` marker from `test_empty_index`. I’ll also add coverage for re-indexing from a non-empty corpus to an empty one so stale BM25 state cannot be reused.

For verification, I’ll rerun the empty-index regression test, the full `tests/unit/test_keyword_search.py` file, and the repository’s required checks. I expect `index([])` followed by a search to return `[]` without raising, while leaving non-empty BM25 behavior unchanged.

---

## Your branch

**Branch**

`fix/68-empty-index`

**Evidence**

### Before

Command:

```text
python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv --runxfail
```

Output:

```text
============================= test session starts =============================
platform win32 -- Python 3.11.13, pytest-9.1.1, pluggy-1.6.0 -- C:\CodePath\AI 301\Unit 3\pathreview-ai301-fa26-s3\.venv\Scripts\python.exe
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0
rootdir: C:\CodePath\AI 301\Unit 3\pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: anyio-4.15.1, hypothesis-6.168.5, platformdirs-4.12.3, asyncio-1.4.0, benchmark-5.3.0, cov-7.1.0, pytest_httpserver-1.2.0
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index FAILED [100%]

================================== FAILURES ===================================
____________________ TestKeywordSearcher.test_empty_index _____________________

self = <tests.unit.test_keyword_search.TestKeywordSearcher object>
searcher = <rag.retriever.keyword_search.KeywordSearcher object>

    @pytest.mark.xfail(
        strict=True,
        reason="issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index",
    )
    def test_empty_index(self, searcher):
        """Test searching on empty index."""
>       searcher.index([])

tests\unit\test_keyword_search.py:140:
rag\retriever\keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv\Lib\site-packages\rank_bm25.py:83: in __init__
    super().__init__(corpus, tokenizer)
.venv\Lib\site-packages\rank_bm25.py:27: in __init__
    nd = self._initialize(corpus)
.venv\Lib\site-packages\rank_bm25.py:52: in _initialize
    self.avgdl = num_doc / self.corpus_size
E   ZeroDivisionError: division by zero

=========================== short test summary info ===========================
FAILED tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index - ZeroDivisionError: division by zero
============================== 1 failed in 1.78s ==============================
```

### After

Command:

```text
python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -vv
```

Output:

```text
============================= test session starts =============================
platform win32 -- Python 3.11.13, pytest-9.1.1, pluggy-1.6.0 -- C:\CodePath\AI 301\Unit 3\pathreview-ai301-fa26-s3\.venv\Scripts\python.exe
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0
rootdir: C:\CodePath\AI 301\Unit 3\pathreview-ai301-fa26-s3
configfile: pyproject.toml
plugins: anyio-4.15.1, hypothesis-6.168.5, platformdirs-4.12.3, asyncio-1.4.0, benchmark-5.3.0, cov-7.1.0, pytest_httpserver-1.2.0
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 1 item

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [100%]

============================== 1 passed in 0.32s ==============================
```

Additional regression test:

```text
python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_reindex_with_empty_list_clears_previous_index -vv
```

Output:

```text
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_reindex_with_empty_list_clears_previous_index PASSED [100%]

============================== 1 passed in 0.30s ==============================
```

Full keyword-search test file:

```text
python -m pytest tests/unit/test_keyword_search.py -vv
```

Output:

```text
============================= 18 passed in 0.32s ==============================
```

Repository checks:

```text
make check
```

Output:

```text
.venv/Scripts/ruff check .
All checks passed!
.venv/Scripts/black .
All done! ✨ 🍰 ✨
110 files left unchanged.
.venv/Scripts/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
```

Unit test suite:

```text
make test-unit
```

Output:

```text
========================================================== 377 passed, 52 xfailed, 2 warnings in 9.59s ===========================================================
```

## Eval iterations

**Run history**

Run 1:

```text
categories: clear-accept 6/7  scope-creep 4/4  thread-convention 1/2  unbuildable 3/3  wrong-cause 4/4
agreement: 18/20 scored items  (bar: 18/20: PASS)
```

This was my only full eval run, so this is also the score recorded in the submitted `eval-run.txt`.

**Package analysis**

I analyzed `pkg-01`.

My rubric verdict:

```text
reject
```

Gold verdict:

```text
reject
```

The candidate diagnosis says:

> "The `REQUEST_ITEM` tokenizer in `httpie/cli/requestitems.py` is the problem."

But the reproduction evidence says:

> "the request items are never handed to HTTPie's item parser."

That means the proposed diagnosis contradicts the reproduction evidence. The failure happens while argparse is consuming the arguments, before HTTPie's request-item tokenizer receives them. Because my `diagnosis-supported` check requires the proposed cause to be supported by the reproduction evidence, this package fails that check and the overall verdict becomes `reject`.

**Check rationale**

The check from my submitted `rubric.md` is:

> | diagnosis-supported | The plan's diagnosis, the issue description/thread, and the quoted reproduction evidence included in the plan. | Pass when the plan explains the observed failure in a way that is supported by the reproduction evidence and identifies the relevant code path or likely cause without presenting unsupported guesses as facts. The diagnosis does not need to prove every implementation detail before work begins. | required |

I wrote the check this way because I wanted the grader to distinguish between an evidence-backed diagnosis and a technically plausible guess. A plan should connect its proposed cause to what was actually reproduced, but I did not want to require the exact final implementation or complete root-cause proof before coding starts. That is why the check allows a relevant code path or likely cause while still rejecting unsupported claims presented as facts.

**Trade-offs**

The main trade-off in `diagnosis-supported` comes from requiring the diagnosis to avoid:

> "presenting unsupported guesses as facts."

This can reject a plan whose proposed cause later turns out to be correct when the reproduction only proves the symptom and does not isolate the exact root cause. I accept that trade-off because the purpose of the plan is to separate what the evidence currently supports from what still needs to be investigated. If implementation reveals a different cause, that can be recorded later as a deviation instead of presenting an unverified assumption as established fact.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
