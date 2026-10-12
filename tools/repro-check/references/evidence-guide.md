# Evidence guide: where proof lives in a reproduction package

This is the rubric's map. For each proof family it says where to look,
in an eval bundle and in live mode, and what good looks like there.
Every rubric check names one of these families as its evidence.

An eval bundle has five parts, always in this order: the **Repo
facts** block (including the `bug reports:` template line and the
`contribution policy` line), the **Issue** (title, opener, body), the
**Thread highlights**, the **Candidate claim comment**, and the
**Candidate repro report**. In live mode the first three come from
GitHub and the last two from the student's draft file.

## Environment

**Where it lives.**
- Eval: the repro report's `Environment:` line or block. The issue's
  target version/platform is in the Issue body and Thread highlights;
  the `bug reports:` line in Repo facts says what the repo's own
  template asks for.
- Live: the draft's environment line. For the target, read the issue
  body and thread on GitHub (`gh issue view <n> --comments`) and the
  repo's bug-report template under `.github/ISSUE_TEMPLATE/`.

**What good looks like.**
- The record names the version of the software under test and the
  OS/platform, plus any setting the issue says changes the failure
  (driver, backend, shell, build profile). One line is enough.
- The version and platform match what the issue targets, or the report
  says in words how they differ ("on Linux instead of macOS", "on the
  current release, not 1.2"). A silent deviation, such as testing an
  old major version against an issue confirmed on the latest, is not
  a reproduction of the issue.

## Steps

**Where it lives.**
- Eval: the repro report's `Steps:` section and its command blocks.
  The trigger to compare against is in the Issue body (the input,
  command, or action the reporter used) and any maintainer note in
  Thread highlights.
- Live: the draft's steps, read against the issue body and thread on
  GitHub.

**What good looks like.**
- A stranger could re-run it: each input is shown inline (the file
  contents, the config, the exact command) or is public (a released
  version, a public playground link). "My project", a private repo, or
  a config that is not shown means nobody else can re-run it.
- It hits the issue's trigger: compare the report's input with the
  issue's input token by token. The same syntax, the same operator,
  the same kind of value. An input changed until something failed is
  a different bug.

## Behavior shown

**Where it lives.**
- Eval: the output blocks, logs, error text, exit codes, and
  measurements inside the repro report, plus its `Expected:` and
  `Actual:` lines. The symptom to compare against is in the Issue
  body.
- Live: the same inside the draft. For the symptom, read the issue
  body and any logs attached on GitHub.

**What good looks like.**
- There is at least one artifact from the author's own run, not just
  sentences describing one.
- The artifact shows the issue's symptom itself: the same error
  message, the same exit code, the same wrong output. Read the
  artifact before reading the narration; if the narration says
  "crash" but the artifact shows a clean error and exit 1, believe the
  artifact.
- A control run (the input that works) next to the failing one is the
  strongest form, but not required.
- For a cannot-reproduce, the artifact shows the real attempt and what
  it actually printed.

## Honesty

**Where it lives.**
- Eval: every sentence in the claim comment and the repro report that
  states a result, a cause, a certainty, or a scope ("confirmed",
  "the root cause is", "guaranteed reproducible", "also happens on").
  Match each to an artifact in the report.
- Live: the same sentences in the draft(s).

**What good looks like.**
- Each claim has an artifact behind it, or is worded as a guess ("I
  think", "may", "likely").
- The stated outcome matches the artifacts. An honest cannot-reproduce
  that shows the attempt and names what may differ is a good report.
  A confident "reproduced" over an artifact that shows something else
  is not.
- The report does not widen the bug beyond what was shown (other
  versions, other platforms, a cause nobody tested).

## Comms

**Where it lives.**
- Eval: the Candidate claim comment read against the Issue; the
  `contribution policy` line in Repo facts read against both
  comments.
- Live: the draft claim comment against the issue on GitHub; the
  repo's `CONTRIBUTING.md`, any AI policy file it links to
  (`AI_POLICY.md`, `AI_USAGE_POLICY.md`), and the PR/issue templates.
  Also the house rules in `scope.md`.

**What good looks like.**
- Specific: the claim names something only this issue has (symptom,
  component, version, input) and one concrete next step. Swap in
  another issue's title: if the comment still makes sense, it is
  boilerplate.
- Modest: it promises investigation and a report, never a fix, a
  deadline, or a guarantee, and does not ask to reserve the issue.
- Disclosed where required: read the policy line for the word that
  matters. "Must disclose" or "state the tool used" means the comment
  has to say AI was used and how. "Understand it", "take
  responsibility", or "write comments in your own words" are
  conditions, not a disclosure requirement. Silence is a pass.
