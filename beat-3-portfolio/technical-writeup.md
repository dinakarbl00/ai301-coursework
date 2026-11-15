# Opening My First Pull Request

## Overview

I wanted to make a real open-source contribution, but finding an issue that matched my skills and was realistic to complete turned out to be harder than I expected. I started by building an issue-scout tool and improving its evaluation rules after the first results weren't useful. That eventually led me to issue #1261 in the JWST Data Analysis repository, where a Python function had unnecessary imports inside it. I investigated the code, wrote a test to confirm the problem, made the changes, and checked that the function still behaved the same way. I submitted the fix as [PR #2180](https://github.com/Snoww3d/jwst-data-analysis/pull/2180). The PR is still awaiting maintainer review, with its GitHub PR Standards check passing. More than the code change itself, this experience taught me how much planning, testing, and verification go into contributing to an unfamiliar project.

## What happened


I started by using my issue-scout tool to find an open-source issue I could realistically work on. I wanted something related to Python that was small enough to complete but would still teach me how to contribute to an existing codebase. My first attempt didn't go as planned. The tool found several possible issues, but its evaluation rules were too strict, so none of them were accepted.

**Evidence from my first issue-scout run:**
- Issues found: 104
- Unassigned candidates: 78
- Issues evaluated: 5
- Issues accepted: 0

When I looked into why this happened, I realized my tool was checking only the five most recent commits to decide whether a repository was actively maintained. This was a problem because automated commits could make an active repository look inactive. I changed the rule to consider human-authored commits from the previous 90 days instead. After making that adjustment, the tool accepted issue #1261 from the JWST Data Analysis repository.

This was my first important lesson: just because a tool produces a result doesn't mean its evaluation criteria are correct. I had to understand why it was rejecting candidates instead of simply trusting its output.

The issue itself was about a Python function called `_table_to_dict_list()` in `mast_service.py`. The function converts astronomical data tables into dictionaries, but it had some imports inside the function that belonged at the top of the file. The `math` library was also being imported twice.

At first, the change looked straightforward. However, I didn't want to move a few lines of code without understanding whether it could affect the function's behavior. I inspected the code using Python's AST module, which allowed me to identify the imports and where they appeared.

I then wrote a regression test to check that the required libraries were imported at the module level rather than inside the function.

**Evidence from the regression test before my fix:**

`Missing module imports: {'numpy', 'time'}`

The test failed on the original code, which confirmed that it could detect the specific problem I was trying to solve.

Once I had that evidence, I moved the `time` and NumPy imports to the top of the file and removed the unnecessary local imports. I deliberately left the actual table-conversion logic unchanged.

I reran the same regression test after the modification.

**Evidence after the fix:**

`1 passed`

I also ran Ruff for Python linting and a Python syntax check. Both passed. To make sure the change hadn't affected the function's behavior, I compared the original and modified versions using four different inputs, including empty data, missing values, and a case that forced the function's fallback conversion path. All four comparisons produced matching results.

Not everything worked perfectly. I tried running three existing MAST test files, but they couldn't even begin executing because my local environment was missing the `astroquery` dependency. Rather than reporting those tests as successful, I documented the limitation in my pull request.

Another challenge was preparing the contribution for submission. My first PR precheck identified missing information about the tests I had attempted. I updated the documentation and reran the check. I also encountered a file-encoding issue that prevented my ship-gate from reading its configuration correctly. Once I corrected the encoding, the gate successfully ran all three required checks.

**Evidence from the final ship-gate:**

- `repo check 1/3 Python lint: ok`
- `repo check 2/3 MAST regression test: ok`
- `repo check 3/3 Python syntax: ok`
- `ship-gate: PASS`

With those checks complete, I committed my changes and opened [Pull Request #2180](https://github.com/Snoww3d/jwst-data-analysis/pull/2180). The pull request includes the import cleanup, the regression test, and an implementation plan explaining the change.

I also set up a PR-monitor tool to track reviews and GitHub checks. Since my contribution was newly submitted, its first recommendation was to wait rather than send an unnecessary follow-up message.

As of November 14, 2026, the pull request is still open with no maintainer review. Its GitHub PR Standards check has passed, although that check does not run the full application test suite.

For me, the biggest result was getting through the entire contribution process, from improving my issue-selection tool and reproducing a problem to writing tests, handling failed checks, and opening a real pull request. The actual code change was small, but understanding how to make and verify that change responsibly took much more effort than I expected.

## Limitations and what's next

One limitation of my contribution is that I wasn't able to run the complete test suite. Some of the existing MAST tests required the `astroquery` dependency, which wasn't available in my local environment. Although my regression test passed and the original and updated functions produced matching results across the cases I tested, I can't claim that the entire application was tested. If I continued working on this project, my next step would be to set up the full development environment and run the remaining tests.

Another limitation is that my issue-scout tool still depends on the rules I give it. Changing the maintainer-activity check improved its results, but that doesn't mean it will evaluate every repository correctly. I'd like to test it against more repositories and compare its recommendations with actual contribution outcomes so I can make its decisions more reliable.

My pull request is also still awaiting maintainer review, so I don't yet know whether the maintainers will accept the change or request modifications. I'll continue monitoring it and respond if feedback comes in.

Looking back, I learned that writing the code is only one part of contributing to an existing project. Understanding the problem, testing my assumptions, documenting limitations, and being open to feedback are equally important. The next time I contribute, I want to spend more time setting up a complete testing environment early, so I can focus more on the actual solution and less on resolving environment issues.