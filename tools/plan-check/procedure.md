# Procedure: how to grade an implementation plan

## Read order

1. Read the issue title, body, and relevant thread comments to understand the requested behavior and any maintainer guidance.
2. Read the reproduction evidence quoted in the plan and identify the observed failure, expected behavior, and affected path.
3. Read the full `plan.md`, including diagnosis, scope, files to touch, implementation approach, test plan, risks, unknowns, and deviations if present.
4. Read the draft plan comment last and compare it with the fuller plan.

## Evidence gathering

1. Identify the exact behavior that was reproduced and the evidence supporting it.
2. Find the plan's explanation of why that behavior occurs and note which code path or files it points to.
3. Identify what the plan says will change and what it explicitly says will stay out of scope.
4. Record every file the plan expects to touch and why each file is needed.
5. Identify the proposed verification steps and compare them with the original reproduction steps.
6. Check the issue thread and repository guidance for requirements or decisions that affect the plan.
7. Compare the draft comment with the detailed plan and note any claim that is broader or more certain than the plan supports.

## Check execution

1. Grade `diagnosis-supported` using only the issue, reproduction evidence, and diagnosis written in the plan. Do not reward a diagnosis just because it sounds technically plausible.
2. Grade `scope-bounded` by checking whether the proposed work serves one clear outcome and whether unrelated refactors or features are excluded.
3. Grade `test-plan-proves-fix` by asking whether the planned test would demonstrate that the reproduced failure is gone through the real affected code path.
4. Grade `thread-and-conventions` by comparing the plan comment with the issue thread, repo guidance, and the detailed plan.
5. For each check, return `pass`, `fail`, or `unclear`, and include a short evidence-based reason.
6. Use `unclear` when the plan does not contain enough information to judge the check safely. Do not invent missing details.

## Verdict assembly

1. Review the grade for every required check.
2. If every required check is `pass`, return `accept`.
3. If any required check is `fail` or `unclear`, return `reject`.
4. Summarize the main reason for the verdict in one or two sentences.
5. End with the structured JSON block required by the skill.