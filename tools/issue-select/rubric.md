# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: last 5 default-branch commits, including dates and authors. | At least 2 of the last 5 default-branch commits are from the last 90 days and represent human maintainer activity. Bot-only automated commits do not count unless they merged a human pull request. | required |
| repo-in-use | Repo facts: archived status, latest release date, and last push to any branch. | The repository is not archived, and either its latest release is within 18 months of the capture date or its last push to any branch is within 90 days. | required |
| scope-fits | Issue title and body, comment thread, labels and opener role, plus linked or mentioned past PR attempts. | Pass when the issue describes one coherent user-visible or developer-visible outcome that a contributor can work toward. Multiple files, supporting edits, optional follow-up work, possible root causes, or suggested technical approaches may all belong to the same outcome. A bug opened by an Owner, Member, or Collaborator with a clear observed failure and a finite set of diagnosed or suspected causes can pass even when the exact implementation approach is not chosen yet; investigation can be part of the contribution. A short issue can also pass when the requested behavior is understandable. Reject when the issue is explicitly an umbrella or tracking issue, combines independent outcomes that could reasonably be completed separately, is only a usage/support question, or the issue/thread shows that a major product or design decision about the desired behavior itself still needs maintainer resolution. Also reject when the history shows at least 2 abandoned or closed-unmerged implementation attempts and there is no maintainer statement showing that the disputed scope or design is now settled. | required |
| unclaimed | Repo facts for assignees and linked PRs, plus claim comments in the issue thread. | The issue has no assignee, no open linked or mentioned PR, and no claim from another contributor within the last 30 days unless the thread clearly says the issue is available again. Closed or abandoned PRs do not count as active claims. | required |
| ai-policy | Repo facts: contribution policy, including CONTRIBUTING.md, linked contributor documentation, dedicated AI policy files, and template requirements. | Pass if there is no explicit ban on AI-assisted or AI-generated contributions. Silence passes. Requirements such as disclosure, testing, personal understanding, or human review also pass. An explicit statement that AI-generated code or documentation is not accepted fails. | required |
| maintainer-response | Repo facts: maintainer first-response sample from recently updated issues. | At least one sampled issue received a maintainer response within 30 days. | preferred |


## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only if every required check passes. A fail or unclear result on any required check rejects the issue. Preferred checks never change the accept or reject verdict; they only help rank issues that already pass.
