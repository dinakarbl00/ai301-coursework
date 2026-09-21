# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

Summary

- maintainer-alive: pass — all 5 recent default-branch commits (Aug 24–Sep 16, 2026) are human commits by Andrew Burke, within 90 days.
- repo-in-use: pass — not archived; last push 2026-09-16, within 90 days.
- scope-fits: pass — single coherent bug (ZeroDivisionError on empty corpus), clear fix location (keyword_search.py + one test file), opened by a Collaborator with a diagnosed cause and effort estimate.
- unclaimed: pass — no assignee, no linked PR; the one comment is a fellow student's claim, which the Path Review house rule says doesn't block.
- ai-policy: pass — docs/CONTRIBUTING.md has no AI-ban language; silence passes.
- maintainer-response (preferred): fail — no maintainer reply on this issue's thread yet (only the student's own claim comment).


**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
  "checks": [
    {
      "name": "maintainer-alive",
      "grade": "pass",
      "evidence": "5/5 of last default-branch commits (2026-08-24 to 2026-09-16) authored by human maintainer Andrew Burke, all within 90 days"
    },
    {
      "name": "repo-in-use",
      "grade": "pass",
      "evidence": "repo.archived=false, pushed_at=2026-09-16T21:50:20Z (within 90 days of 2026-09-20)"
    },
    {
      "name": "scope-fits",
      "grade": "pass",
      "evidence": "Issue body describes one outcome: 'index() shouldn't raise on an empty corpus either', with named files and a 2-4h estimate; opened by author_association COLLABORATOR"
    },
    {
      "name": "unclaimed",
      "grade": "pass",
      "evidence": "assignees: [], no linked PR; only comment is a student claim (author_association NONE) which house rule says doesn't block"
    },
    {
      "name": "ai-policy",
      "grade": "pass",
      "evidence": "docs/CONTRIBUTING.md contains no AI-generated-content ban; only style/PR/CI requirements"
    },
    {
      "name": "maintainer-response",
      "grade": "fail",
      "evidence": "issue's only comment (2026-09-19) is a student claim, no maintainer reply present"
    }
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `agreement: 2/3 scored items`
2. `agreement: 0/1 scored items`
3. `agreement: 0/1 scored items`
4. `agreement: 1/1 scored items`
5. `agreement: 15/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)`
6. `agreement: 3/5 scored items`
7. `agreement: 2/3 scored items`
8. `agreement: 0/1 scored items`
9. `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

I analyzed `issue-19`, which my final rubric marked `accept`, matching the gold label of `accept`. The issue describes one clear performance problem: “Selecting large subgraphs in proof mode freezes the UI,” and then gives two potential causes that should be fixed. It also lists several implementation suggestions, but those are all ways to solve the same UI-freezing problem rather than separate unrelated tasks. :contentReference[oaicite:0]{index=0}

My earlier version of `scope-fits` treated the different possible causes and implementation approaches as unresolved scope, so it rejected this issue. I revised the check to distinguish an unclear desired outcome from a clear bug that still requires technical investigation. That matched the gold rationale, which describes `issue-19` as a “maintainer-diagnosed performance bug with named causes, unclaimed.” :contentReference[oaicite:1]{index=1}

**Check rationale**

I focused most on the `scope-fits` check. Its current pass condition is:

> “Pass when the issue describes one coherent user-visible or developer-visible outcome that a contributor can work toward. Multiple files, supporting edits, optional follow-up work, possible root causes, or suggested technical approaches may all belong to the same outcome. A bug opened by an Owner, Member, or Collaborator with a clear observed failure and a finite set of diagnosed or suspected causes can pass even when the exact implementation approach is not chosen yet; investigation can be part of the contribution. A short issue can also pass when the requested behavior is understandable. Reject when the issue is explicitly an umbrella or tracking issue, combines independent outcomes that could reasonably be completed separately, is only a usage/support question, or the issue/thread shows that a major product or design decision about the desired behavior itself still needs maintainer resolution. Also reject when the history shows at least 2 abandoned or closed-unmerged implementation attempts and there is no maintainer statement showing that the disputed scope or design is now settled.”

I use the issue title and body, comment thread, labels and opener role, and linked or past PR attempts as evidence because those show whether the work is actually bounded and whether earlier attempts exposed unresolved design problems. I chose this threshold because I did not want to reject a good issue just because it touched several files or required investigation, but I still wanted to filter out tracking issues, unrelated bundles of work, and issues with a history of unresolved implementation attempts.

**Trade-offs**

I chose to make the required checks focus on clear blocking risks: inactive repositories, broad or unresolved scope, active claims, and AI contribution bans. I kept maintainer response time as a preferred check instead of a required one because an otherwise healthy repo can still have slow replies on a small sample of issues. The trade-off is that my rubric may accept an issue where getting feedback takes longer, but it avoids rejecting a good, active, well-scoped issue only because recent response times were inconsistent.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. This issue fits my interests because it is a focused Python debugging task and also involves updating a related test. The root cause is already narrowed down to the empty BM25 index case, so the 2–4 hour estimate feels realistic for the time I have available.

2. The verdict correctly identified that the repository is active, the issue has a bounded scope, there is no assignee or linked PR, and there is no AI contribution ban. Outside of the rubric, I also considered my own familiarity with Python and whether I would learn something useful from debugging the failure and validating the fix with a test.

3. I expect claiming it may have a little competition because another student has already commented that they are interested in the issue. However, there is currently no assignee or linked PR, so I will follow the Unit 2 claiming process and be prepared to choose another accepted issue if it is no longer available.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
