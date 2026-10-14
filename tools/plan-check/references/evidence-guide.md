# Evidence guide: where plan evidence lives

## Diagnosis

Where it lives:
- The issue title, body, and relevant thread comments.
- The reproduction evidence quoted or summarized inside `plan.md`.
- The diagnosis section of `plan.md`.

What good looks like:
The plan connects the observed failure to a specific code path or likely cause using evidence from the reproduction. The explanation should be specific enough to guide implementation, but it does not need to prove every internal detail before coding begins. Guesses should be labeled as risks or unknowns rather than stated as facts.

## Scope

Where it lives:
- The scope section of `plan.md`.
- The files-to-touch section.
- The implementation approach.
- Maintainer comments or issue requirements that narrow or expand the work.

What good looks like:
The plan clearly states what will change and what will not. Each proposed file supports the issue's requested outcome. Unrelated cleanup, refactors, or new features should be excluded unless the issue or thread requires them.

## Test plan

Where it lives:
- The Unit 2 reproduction evidence included in the plan.
- The test-plan section of `plan.md`.
- Existing tests or named test files referenced by the plan.

What good looks like:
The planned verification exercises the same real code path that reproduced the bug. It states what command or test will be rerun and what result should change after the fix. A strong test plan distinguishes a real fix from simply bypassing the failing path.

## Thread and conventions

Where it lives:
- The issue thread.
- Repository contribution guidance.
- The draft plan comment.
- The detailed `plan.md`.

What good looks like:
The comment accurately summarizes the plan, matches any maintainer guidance in the issue, and follows repo-specific contribution rules. It should not claim more certainty or broader scope than the detailed plan supports.

## Risks and unknowns

Where it lives:
- The risks/unknowns section of `plan.md`.
- Any unresolved questions in the issue thread or reproduction evidence.

What good looks like:
The plan identifies meaningful uncertainties that could affect implementation, testing, or scope. Unknowns are stated honestly rather than silently filled in. A risk does not make the plan bad by itself if the plan explains how it will be checked during implementation.

## Deviations

Where it lives:
- The `## Deviations` section at the end of `plan.md`.

What good looks like:
Before implementation, this section may be empty or marked as pending. After the build, it should state what changed from the posted plan and why. If nothing changed, it should explicitly say so in the author's own words.