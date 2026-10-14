---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Decide whether one PR package is ready to submit.

A PR package contains a title, description, commits, code diff, and test evidence. Review these against the issue, the approved plan, any documented deviations, and the repository's requirements.

Grade exactly one PR package per run. Do not review unrelated work or invent requirements.

## Inputs and modes

Choose the mode based on the supplied input.

**Live mode:** Review the student's own proposed PR before submission.

Read:
- `plan.md`, including the original plan, test plan, and any deviations.
- The committed branch changes using `git diff main...HEAD`, run from the repository's working directory.
- The PR title and description from `pr_draft.md`.
- Test results, before-and-after reproduction, and commands from `test_evidence.md`.
- The issue URL and its full discussion thread, including the plan comment.
- The repository's `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.

Compare the actual changes with the plan, tests, issue requirements, and PR description. For a house-chain submission, use the supplied house issue, plan, reproduction package, and implementation branch instead of the student's corresponding artifacts. Apply the same rubric.

**Eval mode:** Review one supplied practice PR bundle. The bundle is the entire evidence source. Do not fetch GitHub data, read local repository files, or introduce external facts. Evaluate every rubric check and apply the complete verdict rule, even when the package looks clearly acceptable or unacceptable.

Never mix evidence between the two modes.

## The scope seam (live mode only)

Read `scope.md` first, before collecting any other live-mode evidence.

Confirm the repository matches the `Repo:` entry and follow the house rules listed in the file.

If the entry is missing or still contains a placeholder, stop without grading. Tell the student to fill in the `Repo:` line with their section's Path Review repository.

If the submission belongs to another repository, refuse to grade it and explain the scope mismatch.

Eval mode ignores `scope.md` entirely.

## The voice seam (live mode only)

Read `voice-guide.md` and compare the outgoing PR title and description against its writing rules.

Report any violations in the human-readable summary and suggest specific corrections.

A voice-guide violation does not change the verdict by itself. It affects the verdict only when a check explicitly defined in `rubric.md` makes that requirement relevant.

Eval mode ignores `voice-guide.md` entirely.

## Component reads

Read the installed components at their required paths:

1. `rubric.md` defines each check, its required evidence, pass condition, weight, and the final verdict rule.
2. `references/evidence-guide.md` explains where each kind of evidence is found and what counts as sufficient evidence.
3. `procedure.md` defines the order for gathering evidence, executing checks, and assembling the verdict.

Execute the procedure as written. Apply the rubric to the evidence found using the evidence guide.

If the procedure omits an essential step, report the gap instead of silently creating a new procedure.

If `rubric.md` or `procedure.md` has no substantive written content, refuse to grade. Explain which component must be completed. Template comments do not count as content.

Do not invent checks or pass conditions during a run.

## Verdict and output

Return only one of two verdicts:

- `accept`: the PR satisfies the rubric's submission requirements and is ready to submit.
- `reject`: the PR does not satisfy the verdict rule and should be held for fixes.

Apply the exact verdict rule in `rubric.md`. Do not use a third verdict, numeric score, or conditional acceptance.

Before the JSON, provide a short readable summary of the checks, important evidence, and any fixes needed.

End every completed grading response with the following fenced JSON block. Use the PR URL or bundle ID as `item`, include each rubric check with its grade and one-line evidence, and set the verdict to `accept` or `reject`.

The JSON block must be valid, must follow this schema exactly, and must be the last output with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first:** Every grade must cite a specific fact, file, command result, diff detail, or quote from the available PR package. Never grade based on impressions.
- **Grade substance, not polish:** A short but complete PR can pass. A polished description cannot compensate for missing tests, unexplained changes, or mismatched evidence.
- **Follow the rubric:** Apply each check's stated pass condition and weight consistently. Do not change a grade because the final result feels wrong.
- **Follow the procedure:** Use the steps in `procedure.md` exactly. Report missing guidance instead of improvising.
- **Handle uncertainty:** Mark a check `unclear` when the available evidence cannot establish its outcome. Apply the rubric's verdict rule to that grade. If the rule is silent, treat unverifiable claims as failing the check.
- **Compare claims with artifacts:** Check the description against the actual diff, the diff against the plan, and the testing claims against the recorded results.
- **Do not conceal shortcomings:** Treat disclosed failures and plan deviations according to the rubric rather than automatically rejecting them or silently assuming success.
- **Stay within the evidence:** In eval mode especially, do not assume missing context or use outside knowledge to rescue a package.
