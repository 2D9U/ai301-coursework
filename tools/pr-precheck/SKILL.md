---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: **is this pull request ready
to submit?** A PR package is a candidate pull request (its title,
description, commit list, diff, and test evidence), read against the
plan it claims to implement (including that plan's deviation notes)
and the issue that plan belongs to (its thread and the repo's stated
PR asks and contribution policy). Do not grade the plan itself, the
issue, or whether the fix is the best possible one; grade whether this
PR delivers its plan, proves it, is reviewable, and meets the repo's
stated asks. Grade exactly one package per run.

## Inputs and modes

Decide the mode first.

- **Eval mode**: the input is a package bundle (a markdown file with
  `Repo facts`, `Issue`, `Thread highlights`, `Plan context`, and
  `Candidate PR` sections). The bundle is the whole world: take every
  fact from the bundle text, fetch nothing, read no other file, and
  run every check with the full verdict rule. Ignore `scope.md` and
  `voice-guide.md` entirely.
- **Live mode**: the input is the student's own PR, checked from the
  top folder of their fork's clone on their fix branch. Read exactly
  these inputs:
  1. `plan.md` in the working copy, including its `## Deviations`
     section. (House-chain students: the house plan and house repro
     pack instead; the same checks apply.)
  2. The branch's diff: run `git diff main...HEAD` (three dots) and
     `git log --oneline main..HEAD` in the working copy. Only committed
     changes count; ignore uncommitted files and the untracked drafts
     (`plan.md`, `comment.md`, `pr_draft.md`, `test_evidence.md`) as
     diff content.
  3. The draft PR title and description: `pr_draft.md` (first line is
     the title, the rest is the description).
  4. The test evidence: `test_evidence.md`, plus whatever before/after
     output the description itself pastes.
  5. The issue named in the request: its body and comment thread
     (via `gh issue view <n> --comments`, the GitHub API, or the web).
  6. The repo's stated asks: `.github/PULL_REQUEST_TEMPLATE.md` and
     `docs/CONTRIBUTING.md` in the working copy.
  If the request names no issue URL, or `plan.md` or `pr_draft.md` is
  missing, stop and say which input is missing instead of grading.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. If its `Repo:`
line still holds a placeholder (anything in brackets, or not an
`owner/repo` name), stop without grading and tell the student to fill
the `Repo:` line in `scope.md` with their section's Path Review repo.
If the issue URL is not in the scoped repo, refuse to grade and say
why. Apply the house rules it lists as part of how evidence is read
(for example: the template is always used; one PR per issue; a
classmate's PR on the same issue does not block this one). Never guess
a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after grading the checks. Hold the
outgoing text, the PR title and description in `pr_draft.md`, against
each rule. In the readable summary, list each rule the draft breaks,
quoting the rule's name and the offending sentence, with a suggested
rewrite. The voice guide never changes a check grade or the verdict on
its own: it is the student's personal standard, and only the rubric
decides readiness. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read these files from this skill directory, in this order:

1. `rubric.md`: the checks table (name, evidence, pass condition,
   weight) and the verdict rule. It decides **what** passes.
2. `references/evidence-guide.md`: the map of **where** each evidence
   family lives in a bundle and in live mode, and what good looks like.
3. `procedure.md`: the operating steps. Execute it as written, stage
   by stage: read order, evidence gathering, check execution, verdict
   assembly. It decides **how** and **when**.

When the procedure is silent on a step you need, do not improvise
around it: take the most literal reading of the rubric, and name the
gap in the summary under "Procedure gaps". If `rubric.md` has no filled
check rows or no verdict rule, or `procedure.md` has no written steps
(instruction comments do not count), refuse to grade and say which
file is empty: an empty tool must not invent checks.

## Verdict and output

The verdict is binary: `accept` means ready to submit; `reject` means
hold. There is no third verdict and no score; reservations go in the
evidence lines. Before the JSON you may print a short readable summary:
one line per check with its grade, the deciding check for a reject,
and in live mode the voice-guide notes and any procedure gaps. Then end
the reply with this fenced JSON block, valid, with every rubric check
in rubric order, and nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

In eval mode, `item` is the bundle id (e.g. `pkg-07`); in live mode it
is the issue URL (or the PR URL if one exists).

## Grading discipline

- Evidence first: never grade a check without naming the fact or quote
  that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff itself against the
  plan, and the evidence itself against the test plan. A confident,
  well-formatted description proves nothing the diff or output does
  not show; a terse PR can be ready.
- The rubric decides, not the run: if a check passes by its stated
  condition but feels wrong, it passes. Note the tension in the summary.
- The procedure decides how, not the run: follow `procedure.md` as
  written and report its gaps.
- Unclear defaults to fail: apply the rubric's verdict rule to
  `unclear`; where it is silent, an unverifiable claim is a failing
  one. A PR you cannot verify from the package is not ready to submit.
- Honest shortfalls are not failures: work the plan or its deviation
  notes explicitly defer, and the description restates, is not drift.
