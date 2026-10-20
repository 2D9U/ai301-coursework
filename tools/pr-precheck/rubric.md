# Rubric: is this pull request ready to submit?

Nine checks. Seven are `required` and gate the verdict; two are
`preferred` and never change it. Each check reads one part of the PR
against one other part, so a failed check names exactly what to fix.
The required checks map onto the four ways a PR gets held: silent drift
(checks 1-2), not tested (checks 3-4), unreviewable (check 5), and
standards wall (checks 6-7).

An honestly disclosed shortfall is not a failure: work the plan or its
deviation notes explicitly defer, and the description says is left
out, never counts against the PR. Length, headings, commit count, and
polish are never evidence.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff stays inside the plan | Every changed file and every hunk in the diff, read against the plan's files, scope (in / not in scope), and deviation notes. | Every hunk is part of the planned change, its tests, its stated docs/changelog entry, a marker or suppression the fix makes stale, or a change a deviation note names. Fails when any hunk does something the plan neither names nor records as a deviation: a file the plan never touches, a refactor or rename pass, a new option/flag/setting, a dependency bump, or work the plan explicitly scoped out. Also fails when a planned change is missing from the diff and neither the plan's notes nor the description say it was left out. | required |
| Description matches the diff | Every claim in the PR description about what the PR does or does not do ("implements the plan exactly", "no functional changes outside X", "docs now document Y", "no options added"), read against the diff. | Each claim is true of the diff. Fails when the description claims fidelity or a change the diff contradicts: says "exactly as planned" over extra hunks, says "no changes beyond X" when the diff changes more, or says a file/doc/test was added that the diff does not contain. | required |
| Plan's repro re-run before and after | The test evidence section (live: the description and `test_evidence.md`), read against the plan's test plan and the repro evidence's steps. | Every failure the plan's test plan names is re-run with the **same trigger** as the repro, and the evidence shows its observable result before and after (an exit code, output, error text, value, or byte-for-byte diff), with the after matching the plan's expected result. Fails when the evidence is a claim without an outcome ("tested locally", "works now", "verified for a day"), when it exercises a different input or path than the repro (the control, the unchanged path, a single-file case when the bug needs two), or when it re-runs only some of the failures the test plan names. | required |
| Repo's checks run with results | The test evidence read against the repo-facts block's stated test/check commands (live: the PR template's Testing list and `docs/CONTRIBUTING.md`). | The evidence names the repo's test suite or check command(s) it ran and states their outcome (pass, or an honest fail with a reason). Fails when no suite or check run is mentioned at all, or only "tests pass" with no command named. | required |
| No debris in the diff | The unified diff's added and changed lines, and the commit list. | The diff contains only the change itself. Fails on any of: debug prints or logs left in (`eprintln!("DEBUG`, `print(`, `console.log`), commented-out code (old attempts, disabled lines), dead code (unused functions, `allow(dead_code)`, `_unused` helpers), TODO/FIXME left on touched lines, formatting or re-indent churn on lines the fix does not change, import reshuffles, or identical lines removed and re-added. Explanatory comments about the fix are not debris. | required |
| Repo's stated PR asks met | The repo-facts `pull requests` line (template sections, required checklist items, issue reference, changelog/whatsnew entry) read against the description and the diff. Live: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`. | Every ask the repo states for PRs is met: each named template section has real content, a required checklist is present and filled, the issue is referenced where asked (`Closes #n` / `Fixes #n` / `(#n)`), and a required changelog or whatsnew entry is in the diff. No stated asks passes. Fails when a stated ask is visibly skipped. | required |
| AI use disclosed where required | The repo-facts `contribution policy` line read against the PR description. Every package in this course is treated as AI-assisted work. In live mode Path Review's house rule applies: the course always asks for the disclosure, so the description must disclose. | Apply the **AI use disclosed where required** table below. | required |
| Commits are reviewable | The commit list. | Commit messages describe the change (or follow the repo's stated convention); no "wip", "fix", "misc" messages. | preferred |
| Description engages the thread | The description read against the thread highlights' maintainer signals. | Where a maintainer proposed a direction, asked a question, or named an open PR, the description acknowledges it. | preferred |

### AI use disclosed where required

| Contribution policy says | Grade |
|---|---|
| Nothing about AI, or no policy found (eval mode) | pass |
| AI allowed with conditions that do **not** require disclosure (understand it, test it, take responsibility, write in your own words) | pass |
| Disclosure of AI use **is required** (all AI usage, or in PRs), or live mode in Path Review, and the description says AI was used and how | pass |
| Disclosure of AI use **is required** (all AI usage, or in PRs), or live mode in Path Review, and the description says nothing about AI | fail |
| AI-assisted contributions are banned | fail |

## Verdict rule

**Accept** (ready to submit) only when all seven `required` checks
grade `pass`. Any `required` check graded `fail` **or** `unclear`
produces **reject** (hold): `unclear` counts as `fail`, because a PR
you cannot verify from the package is not ready to submit. `unclear` is
only for evidence that is genuinely absent from the package (for
example, no test-evidence section exists at all); evidence the PR was
responsible for showing and did not is a `fail`.

The two `preferred` checks never change the verdict; they are reported
so a ready PR can still be improved.
