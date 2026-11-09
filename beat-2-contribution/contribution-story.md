# Contribution: Hoist function-local imports in MAST table conversion

**Contribution Number:** [1]
**Student:** Dinakar Balehonniga Lingaraju
**GitHub Username:** dinakarbl00
**Issue:** https://github.com/Snoww3d/jwst-data-analysis/issues/1261
**Pull Request:** https://github.com/Snoww3d/jwst-data-analysis/pull/2180

This story documents my contribution to the JWST Data Analysis project across AI301 Units 5–8. As of November 8, 2026, my pull request is open and awaiting maintainer review.

## Toolkit

**Unit 5: Built and refined an issue scout**

I installed the issue-scout skill, defined a search strategy focused on Python and backend issues, and created a rubric for evaluating contributions. The first scan retrieved 104 issues, identified 78 unassigned candidates, and graded five, but accepted none because my maintainer-activity criterion was too restrictive. I revised the rule to consider human-authored commits over the preceding 90 days rather than requiring human commits among the five newest commits. Regrading issue #1261 then produced an ACCEPT verdict with straightforward difficulty.

**Unit 6: Built contributor safeguards and repository memory**

I installed the voice guard, configured it to inspect GitHub comments, and confirmed that it blocked a body-less GitHub command as intended. I created a local CLAUDE.local.md file that imports AGENTS.md and records only the MAST subsystem, project conventions, and test commands. The reproduction checker accepted my evidence on all six required checks. Plan-check initially rejected my vague test plan and then unsupported wording in my plan comment; I corrected those issues until it accepted all four checks.

**Unit 7: Built and used a ship gate**

I installed ship-gate, wrote a manifest requiring Ruff, the focused regression test, and Python syntax checks, and documented the unavailable Docker suite. My first PR precheck identified missing disclosure of three planned MAST tests; I attempted them, recorded the astroquery collection failures, and updated the draft. A UTF-8 BOM prevented the gate from recognizing its manifest header, so I removed that character without disabling the safeguard. The manual gate then passed all three commands and the live precheck, after which I opened PR #2180.

**Unit 8: Installed a read-only PR monitor**

I installed the PR monitor and wrote a playbook covering responses, staleness thresholds, nudge etiquette, and walk-away criteria. Of the 300 recently merged PRs examined, only one met my outside-contributor filter, so I treated its response time as insufficient evidence of a typical wait and chose a conservative seven-day threshold. The first full monitor run found my PR quiet, with one passing check and no reviews, and drafted a note saying not to nudge yet. Nothing was posted. I also refreshed an expired GitHub CLI authentication token during setup.

---

## Why I Chose This Issue

I wanted my first open-source contribution to be a bounded Python task with a clear outcome I could verify. My issue-scout strategy favored Python, backend services, and testable maintenance fixes over broad refactors or work requiring specialized hardware. The first scan helped me understand the available issues, but its overly strict maintainer-activity rule filtered out otherwise active repositories.

I selected issue #1261, which asks contributors to move function-local imports in the MAST table-conversion helper to module scope. After revising the scout rubric, its verdict was ACCEPT, with straightforward difficulty. The change was small enough to understand, but still required reading the repository conventions, reproducing the reported code structure, designing a regression check, and submitting a real PR. I wanted to practice that complete workflow rather than make an unrelated cosmetic edit.

**Claim comment:** https://github.com/Snoww3d/jwst-data-analysis/issues/1261#issuecomment-6713142875

**Toolkit**

- Tool: My issue-scout skill, with a GitHub CLI search strategy and a scope-feasibility rubric.
- What it produced: An initial candidate scan and an ACCEPT verdict for issue #1261 after regrading.
- Difficulties: The original maintainer-activity check confused recent Dependabot commits with lack of human activity.
- What changed: I updated the rubric to evaluate human commits within 90 days and retained the narrower, testable issue scope.

---

## Understanding the Issue

**Problem Description**

The method `_table_to_dict_list()` converts Astropy tables into dictionaries used by the MAST service. It contained local imports of `math`, `time`, and `numpy`, even though `math` was already imported at module level. The issue was redundant import placement, not a demonstrated runtime failure.

**Expected Behavior**

The module should import `math`, `time`, and `numpy` at module scope, following the existing import conventions. Table conversion should return the same data and preserve the normal and fallback conversion paths.

**Current Behavior**

At baseline commit `0862987`, `math` appeared at module line 10 and was imported again at line 1191. The function imported `time` at line 1192 and `numpy as np` at line 1194. These line numbers differed from the older ones in the issue description because the source file had changed.

**Affected Components**

- `processing-engine/app/mast/mast_service.py`
- `MastService._table_to_dict_list()`
- Focused Python tests under `processing-engine/tests/`

**Toolkit**

- Tool: Local repository memory, Python AST inspection, source inspection, and the repository's AGENTS.md and CONTRIBUTING.md.
- What it produced: An exact diagnosis of the import block and a limited set of files to inspect.
- Difficulties: The line numbers in the original issue were outdated, and a broad repository scan would have used unnecessary time and context.
- What changed: I used AST-based lookup rather than relying on the original line numbers, and kept CLAUDE.local.md focused on the MAST subsystem.

---

## Reproduction Process

**Environment Setup**

I worked on Windows using PowerShell, Python 3.11.13, GitHub CLI, and a fork cloned to `C:\CodePath\AI 301\Final\jwst-data-analysis`. The initial reproduction required only Python's standard-library AST module, so it did not require the full application or Docker services.

I created the branch fix/1261-hoist-mast-imports from baseline commit 0862987. Docker Desktop was available later, although the project's processing container was not running. NumPy, Pandas, Astropy, and pytest were installed locally; astroquery was not.
**Steps to Reproduce**

1. Check out baseline commit `0862987` and inspect `processing-engine/app/mast/mast_service.py`.
2. Use Python's `ast` module to inspect module-level imports and imports inside `_table_to_dict_list()`.
3. Confirm that `math` is imported at module level and locally, while `time` and `numpy` are imported locally.
4. Run the new import-placement regression test against the original code. It fails because `time` and `numpy` are missing at module level.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/dinakarbl00/jwst-data-analysis/commit/0862987 — original source state, before the fix.
- **Screenshots/logs:** AST output showed module-level math at line 10 and local math/time/numpy imports at lines 1191, 1192, and 1194. The baseline regression test reported `Missing module imports: {'numpy', 'time'}`.
- **My findings:** The code structure matched the issue. I did not reproduce a runtime crash because the issue reported a cleanup rather than a functional bug.

The separate Unit 6 repro-check accepted the report on all six required checks.

**Toolkit**

- Tool: Python AST inspection, PowerShell, and the Unit 6 repro-check skill.
- What it produced: Repeatable reproduction evidence with a baseline commit, import locations, and a failing regression test.
- Difficulties: The issue's original line numbers had drifted, and an import-cleanup issue does not have a conventional failing user scenario.
- What changed: I documented the exact code state and explicitly distinguished a code-quality reproduction from evidence of a runtime failure.

---

## Solution Approach

**Analysis**

The function used `math`, `time`, and NumPy during table conversion, but those imports did not depend on function-local values. The extra `math` import duplicated an existing module import. Hoisting `time` and `numpy` would address the reported structure without changing the conversion algorithm.

The main risk was that NumPy would be loaded when the module was imported rather than on the first function call.

**Proposed Solution**

Move `time` into the standard-library import group and `numpy as np` into the third-party import group. Remove the local `math`, `time`, and `numpy` statements. Leave the table-conversion logic, return values, exception handling, URI rewriting, and public interfaces unchanged.

**Implementation Plan**

1. Record baseline imports and add an AST regression test that fails before the fix.
2. Make the import-only change in `mast_service.py`.
3. Rerun the regression test, compare conversion outputs, run relevant repository checks, and document unavailable tests.
4. Submit the implementation plan in `docs/plans/features/1261-hoist-mast-imports.md`.

**Deviations:** Three existing MAST test files could not run because host Python lacked `astroquery`. I documented the collection errors instead of counting them as passing tests. I also added the regression test as planned, although the production fix itself required only a small import diff.

The repository's `/plan-ceo-review` recommended proceeding in Hold Scope mode. CodePath's plan-check ultimately accepted all four checks after I strengthened the test strategy and corrected evidence wording.

**Toolkit**

- Tool: Unit 6 plan-check, repository `/plan-ceo-review`, and the local implementation plan.
- What it produced: A bounded plan with a baseline-failing test, behavioral comparison, lint, syntax checks, and risk assessment.
- Difficulties: Early plan-check runs rejected unnamed tests and then unsupported claims about what had been reviewed or committed.
- What changed: I named the tests and expected outcomes, recorded the review result, and removed claims that exceeded the available evidence.

---

## Testing Strategy

I used red-green testing because this was a code-structure fix. The AST regression test failed on the original version with `Missing module imports: {'numpy', 'time'}` and then passed after the import cleanup.

**Checks that passed:**

- `python -m pytest -o "addopts=" processing-engine/tests/test_mast_import_placement.py -q` — 1 passed, with a pytest configuration warning.
- `python -m ruff check processing-engine` — all checks passed.
- `python -m py_compile processing-engine/app/mast/mast_service.py` — passed.
- `git diff --check` — passed.
- Ship-gate independently reran Ruff, the regression test, and syntax validation successfully.

I also compared the original and modified `_table_to_dict_list()` function bodies with the same Astropy inputs. All four cases produced identical results: `None`, an empty table, a mixed numeric/string table containing NaN and infinity, and a forced fallback after Pandas conversion raised an exception. This comparison used an identical no-op URI helper in both versions, so it did not verify URI rewriting or full-service imports.

**Checks attempted but blocked:**

- `test_mast_facet_search.py`
- `test_mast_pagination.py`
- `test_mast_search_geometry.py`

All three stopped during collection with `ModuleNotFoundError: No module named 'astroquery'` (exit code 2). None of their tests executed. The full Docker pytest suite was also not run because the `jwst-processing` container was not running. I disclosed these limitations in the PR rather than claiming complete test coverage.

**Toolkit**

- Tool: Python unittest/pytest, Ruff, AST checks, behavioral comparison, and the Unit 7 ship-gate.
- What it produced: A failing-before/passing-after regression record, four matching conversion outputs, and a ship-gate PASS.
- Difficulties: Missing pytest plugins initially caused command-line errors; missing astroquery blocked existing test collection. A Windows-generated BOM also prevented ship-gate from parsing its manifest.
- What changed: I used `pytest -o "addopts="` for focused testing, recorded blocked tests explicitly, and removed the manifest BOM without weakening the checks.

---

## Implementation Notes

**November 7, 2026**

I finalized my issue selection, revised the issue-scout activity criterion, and claimed issue #1261. I installed the Unit 6 safeguards and prepared the reproduction workflow.

**November 8, 2026 — Planning**

I cloned my fork, created `fix/1261-hoist-mast-imports`, and wrote the scoped repository-memory file. AST inspection reproduced the issue. Repro-check accepted the reproduction, and the implementation plan passed both the repository review process and CodePath plan-check after revisions.

**November 8, 2026 — Implementation**

I added `test_mast_import_placement.py` and confirmed that it failed on the original imports. I then hoisted `time` and NumPy to module scope and removed the local imports. The new test passed, Ruff and syntax checks passed, and isolated before/after conversion results matched in all four cases.

**November 8, 2026 — PR and monitoring**

I documented the three MAST test collection errors, passed the final PR precheck and ship-gate, and committed the change as `7860431` (`refactor(mast): hoist converter imports`). I pushed the branch and opened PR #2180. I installed the Unit 8 monitor, established my playbook, and ran it once; its recommendation was to wait rather than nudge a newly opened PR.

**Toolkit**

- Tool: Git, GitHub CLI, PowerShell, Claude Code skills, and repository planning/validation tools.
- What it produced: A three-file commit containing the source cleanup, regression test, and implementation plan, followed by upstream PR #2180.
- Difficulties: Windows encoding, missing dependencies, GitHub CLI authentication, and multiple checks that required more specific evidence.
- What changed: I fixed environmental issues without changing the intended import cleanup, and kept local drafts and tool state out of the upstream commit.

---

## Pull Request

**PR Link:** https://github.com/Snoww3d/jwst-data-analysis/pull/2180

**PR Description:** `refactor(mast): hoist converter imports`. The PR moves `time` and NumPy imports to module level, removes redundant local imports, adds an import-placement regression test, and includes the required implementation plan. It references `Closes #1261`, explains the tests and their limitations, documents the import-time risk and rollback, and discloses AI assistance.

**Maintainer Feedback:**

- November 8, 2026: No maintainer comments or reviews yet at the time of the Unit 8 monitor run.
- Response: No reply or nudge posted because the PR was less than a day old.

**Status:** Awaiting review as last checked on November 8, 2026. The PR was open, not a draft, with one passing CI check and no review decision. A merge-blocked state was reported, but no maintainer rejection was recorded.

**Toolkit**

- Tool: Unit 7 pr-precheck and ship-gate; Unit 6 voice guard; Unit 8 PR monitor and playbook.
- What it produced: An accepted precheck, a passing ship-gate, upstream PR #2180, a PR-state report, and a locally drafted wait note.
- Difficulties: The first PR precheck rejected an omission in test reporting; the ship-gate initially misread its manifest because of a UTF-8 BOM; the first monitor attempt encountered invalid GitHub CLI authentication.
- What changed: I documented every planned test, repaired manifest encoding, refreshed authentication, and chose not to post the monitor's premature nudge.

---

## Learnings and Reflections

**Technical Skills Gained**

I practiced Python AST inspection, import organization, focused regression testing, and comparing function behavior across Git commits. I also gained experience with Git forks and branches, conventional commits, upstream PR templates, linting, and the distinction between test failures, test-collection errors, and missing integration coverage.

**Challenges Overcome**

The most difficult part was not moving the imports. It was producing evidence that a maintainer or course reviewer could trust. I had to revise an overly strict issue-selection rubric, strengthen a vague plan, solve Windows hook and encoding problems, and distinguish unavailable tests from passing tests. Each issue made the contribution process more reproducible.

**What I'd Do Differently Next Time**

I would inspect the repository's PR and test requirements before drafting the implementation plan, establish a working test environment earlier, and write the final contribution story as I complete each phase. I would also validate file encoding before running Markdown-based gates. For small fixes, I would continue favoring narrow changes and honest test reporting over claiming broader coverage than I actually ran.

---

## Resources Used

- Issue #1261: https://github.com/Snoww3d/jwst-data-analysis/issues/1261
- Claim comment: https://github.com/Snoww3d/jwst-data-analysis/issues/1261#issuecomment-6713142875
- Pull request #2180: https://github.com/Snoww3d/jwst-data-analysis/pull/2180
- Repository contribution guide: https://github.com/Snoww3d/jwst-data-analysis/blob/main/CONTRIBUTING.md
- Repository agent instructions: https://github.com/Snoww3d/jwst-data-analysis/blob/main/AGENTS.md
- AI301 Unit 5 starter: https://github.com/codepath/ai301-unit5-starter
- AI301 Unit 6 starter: https://github.com/codepath/ai301-unit6-starter
- AI301 Unit 7 starter: https://github.com/codepath/ai301-unit7-starter
- AI301 Unit 8 starter: https://github.com/codepath/ai301-unit8-starter
