# Evidence guide: where evidence lives in a PR package

Use this guide to locate evidence for each check in `rubric.md`. Compare the actual artifacts, not the confidence of the PR's wording. In eval mode, use only the supplied bundle; in live mode, use the named local files, branch diff, issue thread, and repository rules.

## Plan fidelity (harness category: silent-drift)

**Where it lives — eval:** Read `## Issue` and `## Thread highlights` for the requested behavior and constraints. Read `## Plan context` for the agreed scope, named files, test plan, and out-of-scope boundaries. Compare them with `## Candidate PR` → `### Diff`, `### Commits`, and `### Description`.

**Where it lives — live:** Read the issue #68 thread and posted plan comment, then `plan.md`, including `## Deviations` if present. Compare every file and behavioral change in `git diff main...HEAD` against the plan. Verify that the description in `pr_draft.md` describes what the diff really does.

**What good looks like:** Every code change is within the plan's scope or is explicitly justified as a deviation, and every promised change is implemented or honestly identified as omitted. The PR description claims no more and no less than the diff demonstrates. An extra behavior, missing promised piece, or changed file outside the boundary with no explanation is silent drift. A disclosed deviation may preserve fidelity; unrelated cleanup is still unrelated even if disclosed.

## Test evidence (harness category: not-tested)

**Where it lives — eval:** Compare `## Plan context` → reproduction evidence and `Test plan` with `## Candidate PR` → `### Test evidence`. Also consider actual commands and outcomes in the description when evidence is recorded there, but do not mistake a checklist claim for test output. Read `## Repo facts` for any explicitly required checks.

**Where it lives — live:** Use the Unit 2 reproduction instructions/results and the test plan in `plan.md`. Compare them with actual before/after reproduction output and command results in `test_evidence.md`, and with the Testing section of `pr_draft.md`. Look for results from `make test-unit`, `make test-integration`, `make lint`, and `make typecheck` as requested by Path Review's PR template. The integration command may report no tests collected; report that exact outcome rather than claiming it passed.

**What good looks like:** The before state demonstrates the original issue, while the after state directly tests the intended fix and records the observed outcome. Expected results and the commands used are identifiable, and each claimed repository check has a visible result. A check that fails can still be useful evidence when reported accurately and explained; `tests pass` without observable supporting results is not sufficient when a check requires proof. Never infer success from a missing run.

## Diff quality (harness category: unreviewable)

**Where it lives — eval:** Inspect `## Candidate PR` → `### Diff` and `### Commits`, together with the plan's file boundary and the issue. The changed lines themselves are the primary evidence.

**Where it lives — live:** Inspect `git diff main...HEAD` and `git diff main...HEAD --stat` in the fork's working clone on the implementation branch `fix/68-empty-index`. Check each changed file against `plan.md` and the issue. Use the commits for context, not as a substitute for reading the diff.

**What good looks like:** The fix is visible in a focused, reviewable patch, with no unrelated files or changes. Reject or flag debug prints, temporary instrumentation, abandoned/commented-out code, accidental generated files, broad formatting churn, and drive-by refactors that obscure the issue fix. Concision alone is not the criterion: a larger patch can be reviewable when every change is needed and traceable to the issue.

## Standards and comms (harness category: standards-wall)

**Where it lives — eval:** Read `## Repo facts` for PR-template requests and contributing or AI-use policies; inspect `## Thread highlights` for explicit maintainer instructions. Compare these with `## Candidate PR` → `### Title` and `### Description`, including any testing claims, issue links, explanations, and disclosures. Do not invent a requirement that the bundle does not state.

**Where it lives — live:** Read `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and the issue #68 thread. Review the first-line title and all template sections in `pr_draft.md`: Summary, Issue (`Closes #68`), Changes, Testing, Screenshots / Demo, and Notes for Reviewers. Check for an honest AI-use disclosure, appropriate checkbox states, and explanations of deliberate omissions or deviations. Also read `voice-guide.md` for live-mode writing feedback; voice rules affect the verdict only if a rubric check explicitly makes them decisive.

**What good looks like:** The PR addresses every applicable stated requirement with specific information rather than empty headings or untouched boilerplate. The issue is linked, test results are accurately characterized, CI is not claimed before it runs, relevant maintainer directions are addressed, and any AI disclosure required by the repository/course is present. If a template section is inapplicable, `N/A` plus a concise reason is acceptable where the rules permit it. Description-to-diff accuracy is assessed under **Plan fidelity**, not substituted by polished writing here.
