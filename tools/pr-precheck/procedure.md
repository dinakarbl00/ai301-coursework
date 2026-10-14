# Procedure: review one PR package

Follow this procedure for one package per run. Use the checks and verdict rule in `rubric.md`; use `references/evidence-guide.md` to find and interpret evidence. Do not add checks while grading.

## Read order

1. Identify the mode. **Eval mode** is a supplied frozen PR package or bundle ID. **Live mode** is a student's working branch and PR draft.
2. In **live mode**, read `scope.md` first. If `Repo:` is blank or a placeholder, stop without grading and ask for the correct repo entry. If the PR belongs outside that repo, refuse to grade it. Read `voice-guide.md` for feedback on outgoing PR wording.
3. Confirm `rubric.md` and `procedure.md` contain actual written instructions beyond template comments. If either is empty, refuse to grade and name the missing component.
4. Read the complete PR evidence in this order: issue and its discussion; plan and any `## Deviations`; actual diff/commits; draft title and description; test/reproduction evidence; repository requirements. Use the evidence guide to locate each part.
5. In **eval mode**, read only the supplied bundle, including its repo-facts, issue, plan, diff, PR draft, and evidence sections. Do not fetch GitHub pages, run commands, or read external/local files to fill gaps. Ignore `scope.md` and `voice-guide.md`.
6. In **live mode**, read the issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md` from the scoped repository. Read `plan.md`, `pr_draft.md`, and `test_evidence.md` from the working copy; collect committed changes with `git diff main...HEAD` (three dots), run at the top of the working copy. Review the branch's commits when available. If following a house chain, substitute the supplied house issue, house plan, reproduction pack, and branch.

## Evidence gathering

1. Write down the issue's expected behavior, boundaries, and constraints. Record the plan's proposed changes, touched files, reproduction steps, and planned tests. Note each documented deviation with its reason.
2. Inventory each material file and hunk changed in the diff. Pair each one with a plan item, documented deviation, or necessary supporting edit. Identify extra changes, missing planned work, and debugging debris.
3. Pair each material PR claim (title, summary, change bullets, notes) with a supporting diff hunk or test observation. Record claims of work not present, and implementation behavior not acknowledged by the description.
4. Pair the original failure/reproduction evidence with the after-fix observation. Identify the actual command/input and observed result for each; compare recorded testing with the plan's test strategy.
5. List every check the supplied repository standards or PR template require. Pair each with its recorded result or an explicit explanation that it was not run. Distinguish a reported failure from a missing or falsely reported result.
6. Compare the PR text with the repository's required sections, issue linkage, and AI-use disclosure rules. In live mode, separately flag broken `voice-guide.md` rules for the readable summary.
7. For each check, record a concise, specific evidence fact or quote and where it came from. If evidence is absent or contradictory, record that absence or contradiction rather than assuming success.

## Check execution

1. Execute **every** row in the `rubric.md` checks table, in its listed order, even if an earlier required check fails.
2. For each row, compare only the named evidence with its stated pass condition. Assign exactly `pass`, `fail`, or `unclear` and write one evidence line naming the deciding fact, example, or omission.
3. Mark `pass` when the provided artifact demonstrates the pass condition. Mark `fail` when it contradicts that condition. Mark `unclear` when supplied evidence cannot establish the condition one way or the other. Do not upgrade an unclear claim using assumptions.
4. Apply evidence-based comparisons: diff vs. plan; description vs. diff; before/after repro and checks vs. test plan; PR text vs. actual repository standards. Do not grade prose polish in place of these comparisons.
5. Honor the rubric's allowance for honest disclosures: a failed or unrun check, known limitation, or changed plan is not automatically fatal **if** the relevant pass condition explicitly permits it and the disclosure is sufficient. Do not excuse silent drift, unsupported claims, or mandatory policy omissions.
6. If an essential procedure step is unspecified, note the procedural gap; do not invent an additional step or requirement during the run. If evidence guide guidance is missing, note the gap and grade according to the rubric using only available evidence.

## Verdict assembly

1. Apply the verdict rule written in `rubric.md`, without changing thresholds after seeing the package. Under the current rule, return `reject` if any `required` check is `fail` or `unclear`; otherwise return `accept`. A `preferred` check cannot independently change the verdict.
2. Write a brief human-readable per-check summary, including concrete evidence and the most useful fixes for any failed required checks. In live mode, also mention any voice-guide issues without treating them as automatic verdict failures.
3. End with exactly one fenced `json` code block matching the unmodified schema in `SKILL.md` and `CONTRACT.md`. Set `item` to the PR URL in live mode or bundle ID in eval mode. Include every rubric check, in order, with its exact name, grade, and one-line evidence. Set `verdict` to `accept` or `reject`.
4. Ensure the JSON is valid, the verdict matches the per-check grades and rubric rule, and **nothing follows the fenced JSON block**. If a refusal rule was triggered, explain the refusal instead of inventing check results or issuing a verdict.
