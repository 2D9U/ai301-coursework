# Evidence guide: where evidence lives in a plan package

An eval bundle has these sections, in order: `Repo facts`, `Issue`,
`Thread highlights`, `Repro evidence`, `Candidate plan`, `Candidate
plan comment`. In live mode the same evidence lives in: the issue page
(body and comment thread), the student's own posted repro comment on
that issue, the repo's `docs/CONTRIBUTING.md` (and any AI policy
file), and the student's `plan.md` and draft comment.

## Diagnosis and grounding

- **Where it lives**: the cause is in the plan's first paragraph
  (`Cause:`, `Diagnosis:`, `Root cause:`). The evidence it must fit is
  the `Repro evidence` block: numbered steps, artifacts, `Control:`
  lines, intermediate values, and the `Expected:` / `Actual:` lines.
  Live: the student's posted repro comment (Steps, Observed, Control).
- **What good looks like**: the cause explains the failing artifact
  and every control. A control run where the blamed component works
  (same input without the flag, the same method called a different
  way, the pipeline stage before the blamed one already wrong) rules
  the cause out, however confident the plan or thread sounds.

## Scope

- **Where it lives**: the plan's `Change:` / `Scope:` / `In:` / `Out:`
  lines and its numbered approach steps; extra work often hides in
  later steps or "phase 2" / "while I'm here" lines.
- **What good looks like**: one change at the code site the repro
  isolates, plus its regression tests and anything the fix makes stale
  (an `xfail` marker, a lint suppression). Wider work appears only
  under "out of scope" or "follow-up". A drive-by rewrite adds
  refactors, upgrades, migrations, new options, or a redesign to the
  same plan.

## Executability

- **Where it lives**: the files or functions named in the plan's
  change and approach lines (`pkg/...`, `src/...`, a function name).
- **What good looks like**: a named file or code site and one chosen
  approach. A vague plan says "investigate", "profile", "somewhere",
  "whichever is easier", or lists layers without picking one.

## Test plan

- **Where it lives**: the plan's `Test:` / `Test plan:` lines, read
  against the repro's numbered steps and `Expected:` line.
- **What good looks like**: the repro step re-run with a concrete
  expected result (exit 0, the color flips at step 3, a float instead
  of a traceback), or the issue's case added as a named regression
  test. A vague plan says "should feel fast", "nothing should break",
  or only "run the full test suite".

## Honesty

- **Where it lives**: the plan's `Risk:` / `Unknowns:` lines and any
  sentence claiming certainty; in live mode also the `## Deviations`
  section at the end of `plan.md`, where a mid-build change and its
  reason get recorded.
- **What good looks like**: open questions named as open, each with
  what would settle it. False confidence asserts untested behavior as
  fact ("this fixes all cases").

## Comms

- **Where it lives**: the `Candidate plan comment`, read against the
  `Thread highlights` (each line shows the author's role in
  parentheses: OWNER, MEMBER, CONTRIBUTOR, NONE) and against the
  `contribution policy` line in `Repo facts`. Live: the issue's comment
  thread and `docs/CONTRIBUTING.md`.
- **What good looks like**: the comment names what maintainers already
  said (a located culprit, a proposed or rejected approach, a patch, an
  open PR) and follows it or explains the difference. Where the policy
  requires disclosing AI use in comments or "all AI usage", the comment
  says the tool and how it was used. A disclosure requirement placed
  only on the pull request does not apply to the plan comment.
