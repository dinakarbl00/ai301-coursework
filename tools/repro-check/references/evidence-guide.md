# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives:
- Eval mode: the repro report's environment section and the issue context.
- Live mode: the student's draft repro comment, the issue, and the repo's setup documentation.

What good looks like:
The report names the important environment details needed to rerun the reproduction, such as operating system, language/runtime version, dependency or application version, and any setup that affects the bug. Those details should match the issue's target environment, or any difference should be stated clearly.

## Steps

Where it lives:
- Eval mode: the reproduction steps in the report, read with the issue description and repo setup instructions.
- Live mode: the student's draft repro comment and the repository documentation.

What good looks like:
The steps begin from a clear starting state and include the commands or actions needed to reach the behavior being tested. A stranger should be able to follow them in order without having to guess a missing setup step, input, file, or command.

## Behavior shown

Where it lives:
- Eval mode: output excerpts, logs, stack traces, screenshots, test output, or other artifacts included in the reproduction package.
- Live mode: the evidence included with the student's draft repro comment.

What good looks like:
The artifacts come from an attempt that meaningfully tests the behavior reported in the issue. A successful reproduction should visibly demonstrate the reported failure. A cannot-reproduce attempt can also be valid when it shows the observed behavior and clearly records any important environment or trigger difference. Evidence from an unrelated feature, code path, input, or setup failure does not establish anything about the issue being tested.

## Honesty

Where it lives:
- Eval mode: compare the report's stated result with its steps and artifacts.
- Live mode: compare the student's conclusion with the evidence they provide.

What good looks like:
The conclusion says only what the evidence supports. A successful reproduction should be backed by matching evidence. A cannot-reproduce result is also valid when the report clearly says what was tried and shows the observed behavior instead of claiming success.

## Comms

Where it lives:
- Eval mode: the claim comment, repro comment, repo-facts block, contribution policy, templates, and any stated AI-use rules.
- Live mode: the issue thread, repository contribution docs/templates, and the student's draft comment.

What good looks like:
The claim names the specific issue and says what the contributor plans to investigate without promising a fix or deadline. The repro comment explains what was tested and what happened in the contributor's own words. Repository rules are applied to the surface they actually govern. If AI disclosure is explicitly required for issue comments, reproduction reports, or all contributor communications, the applicable comment must disclose it. A disclosure rule limited to code, commits, or pull requests does not automatically apply to an issue comment.
