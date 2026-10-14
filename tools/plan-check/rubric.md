# Rubric: is this implementation plan ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-supported | The plan's diagnosis, the issue description/thread, and the quoted reproduction evidence included in the plan. | Pass when the plan explains the observed failure in a way that is supported by the reproduction evidence and identifies the relevant code path or likely cause without presenting unsupported guesses as facts. The diagnosis does not need to prove every implementation detail before work begins. | required |
| scope-bounded | The plan's scope, files-to-touch section, implementation approach, issue requirements, and any maintainer comments that narrow or expand the work. | Pass when the plan names the intended change and its boundaries clearly enough to tell what will and will not be changed. The proposed files and approach should serve the issue's stated outcome without adding unrelated refactors or features. | required |
| test-plan-proves-fix | The plan's test plan read against the Unit 2 reproduction evidence and the issue's expected behavior. | Pass when the planned verification reruns the reproduced failure through the real affected code and states what should be different after the change. It should include the relevant existing or new test and enough detail to distinguish a real fix from simply avoiding the failing path. | required |
| thread-and-conventions | The draft plan comment, issue thread, repository contribution guidance, issue-specific instructions, and the plan itself. | Pass when the draft comment accurately summarizes the proposed plan, does not contradict the issue thread or repository rules, and does not claim work or certainty beyond what the plan supports. If the thread or repo requires a specific convention for the plan or contribution, the draft follows it. | required |

## Verdict rule

Accept only if every required check passes. A fail or unclear result on any required check rejects the plan. An unclear grade means the available plan or comment does not contain enough evidence to decide that check safely.