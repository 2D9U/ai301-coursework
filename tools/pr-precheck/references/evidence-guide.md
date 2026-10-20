# Evidence guide: where evidence lives in a PR package

An eval bundle has these sections, in order: `Repo facts`, `Issue`,
`Thread highlights`, `Plan context` (the accepted plan plus the repro
evidence it built on), and `Candidate PR` (`Title`, `Description`,
`Commits`, `Diff`, `Test evidence`). In live mode the same evidence
lives in the fork's working copy and on GitHub: `plan.md` (with its
`## Deviations` section), the branch diff from `git diff main...HEAD`,
the commits from `git log --oneline main..HEAD`, `pr_draft.md` (title
on line 1, description below), `test_evidence.md`, the issue thread,
`.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`.

## Plan fidelity (harness category: silent-drift)

- **Where it lives**: the plan's scope, files, and stated limits are in
  `Plan context` (`Scope`, `Files:`, `Not in scope`, deferral or
  "known limit" notes). What actually changed is the `Diff` (the
  `--- a/` / `+++ b/` file headers and each `@@` hunk). Fidelity claims
  are in the `Description` ("implements the plan exactly", "no other
  changes", "docs now cover..."). Live: `plan.md` and its
  `## Deviations`, the branch diff, `pr_draft.md`.
- **What good looks like**: every file in the diff is one the plan
  names (or a test/changelog file for that change), every hunk does
  what the plan says, and every planned item is in the diff or is
  deferred in a deviation note that the description restates. Silent
  drift goes both ways: extra hunks (a second file, a rename pass, a
  new option) or missing promised work (docs the description says were
  updated but no docs hunk exists).

## Test evidence (harness category: not-tested)

- **Where it lives**: the `Test evidence` section of the candidate PR,
  read against the plan's `Test plan` and the repro evidence's trigger
  in `Plan context`. The repo's test commands are in `Repo facts`
  (`pull requests` and `contribution policy` lines). Live:
  `test_evidence.md` and the description's Testing section, read
  against `plan.md`'s test plan; the repo's checks are the ones Path
  Review's PR template lists (`make test-unit`, `make test-integration`,
  `make lint`, `make typecheck`).
- **What good looks like**: the repro's own command re-run on the same
  input, with the before (the failure) and the after (the plan's
  expected result) both shown, for every failure case the test plan
  names; plus the repo's suite or checks named with their outcome.
  Weak evidence says "tested locally", "works now", or runs a
  different input (the control, the path the fix did not touch).

## Diff quality (harness category: unreviewable)

- **Where it lives**: the added (`+`) and removed (`-`) lines of the
  `Diff`, and the `Commits` list. Live: `git diff main...HEAD` and
  `git log --oneline main..HEAD`.
- **What good looks like**: every changed line is part of the fix or
  its test. Debris tells: `DEBUG`/`print`/`eprintln!`/`console.log`
  lines, commented-out code, unused functions or `allow(dead_code)`,
  TODO/FIXME on touched lines, re-indented or re-ordered lines with no
  behavior change, import reshuffles, and commits named "wip", "fix",
  or "misc cleanups".

## Standards and comms (harness category: standards-wall)

- **Where it lives**: the repo's asks are the `Repo facts` lines
  `pull requests` (template sections, required checklist, issue
  reference, changelog or whatsnew entry) and `contribution policy`
  (including AI-use disclosure). Where the PR honors them: the
  `Description` (sections, checklist, `Closes #n`, disclosure) and the
  `Diff` (a changelog/whatsnew file). Maintainer direction is in
  `Thread highlights`. Live: `.github/PULL_REQUEST_TEMPLATE.md`
  (Summary, Issue `Closes #`, Changes, Testing checklist, Screenshots /
  Demo, Notes for Reviewers), `docs/CONTRIBUTING.md`, and the course's
  standing AI-disclosure ask in Notes for Reviewers.
- **What good looks like**: each stated section has real content (not
  the template's placeholder), a required checklist is present and
  ticked honestly, the issue is referenced, a required changelog entry
  is in the diff, and where the policy requires AI disclosure the
  description says what tool was used and how. A visibly ignored ask
  (no checklist where one is required, no disclosure where it is
  required) is a standards wall even when the code is excellent.
