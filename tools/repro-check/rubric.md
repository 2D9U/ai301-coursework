# Rubric: is this reproduction package ready to post?

Eleven checks, one question each. Ten are `required` and gate the
verdict; one is `preferred` and never changes it. Each check reads
**one** part of the package and asks **one** thing, so a failed check
names exactly what to fix. Where a check has to branch, the branch is
a small decision table below the checks instead of stacked and/or
prose.

The first seven checks read the **repro report**. The three claim
checks (Claim names this issue's specifics, Claim promises only
investigation, AI use disclosed where required) read the **claim
comment**, and the disclosure check reads both comments.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment record (usually an `Environment:` line). | The record names **both** the version of the software under test **and** the OS/platform it ran on, plus any setting the **issue itself** says changes the failure (driver, backend, shell, build profile). No record at all fails. One terse line that names these passes. | required |
| Environment deviation acknowledged | The report's environment record read against the version/platform the issue reports or targets (issue body, thread highlights). | Passes when the report ran on the issue's version/platform, **or** the report states the difference in words ("different OS", "current release instead of 1.2"). Fails when the report ran somewhere materially different (an older major/minor version, a platform the issue says matters) and says nothing about it. | required |
| Steps re-runnable by a stranger | The report's steps and commands. | Every input a step needs is shown inline or is public (a released version, a public link, a file whose contents the report shows). Fails when any step depends on something only the author has (a private repo, an unshared config, "my setup"), or when there are no steps at all. | required |
| Steps hit the issue's trigger | The report's commands/inputs read against the triggering input or action the issue describes. | The report runs the **same** trigger the issue names: same syntax, same operation, same kind of input. Fails when the input was changed so it exercises a different code path (a different operator, a different range form, an unbound variable, a syntax error the issue does not describe). | required |
| Artifact present | The report's captured output: terminal excerpts, logs, error text, exit codes, measurements, screenshots. | At least one artifact captured from the author's own run. Prose alone ("I confirmed it", "it crashes for me too", "guaranteed reproducible") fails, however confident. | required |
| Artifact matches the stated outcome | The artifacts read against the issue's described symptom and against the report's own stated result. | Apply the **Artifact matches the stated outcome** table below. | required |
| Claims backed by the artifacts | Every sentence in the claim comment or report that asserts a cause, a certainty, or a scope ("root cause is", "I verified", "also affects X"). | Each such assertion either points to a shown artifact or is worded as a guess ("I think", "may", "likely"). Fails on any assertion of cause, certainty, or wider scope that no artifact supports. A plain report of a variant run the author did, naming its exact change and result ("dropping `-r` gives 1, 4, 7, 10"), is an observation, not an unbacked claim. | required |
| Claim names this issue's specifics | The claim comment read against the issue. | The claim mentions at least one thing only this issue has (its symptom, component, version, or input) **and** one concrete next step. Fails on interchangeable text that would fit any issue ("this looks like a good one for me", "+1", "please assign me"). | required |
| Claim promises only investigation | The claim comment. | The claim promises only what the author will look into or report back on. Fails on a promise of a fix, a guaranteed outcome, or a deadline ("I will fix it within 2 days", "guaranteed"), or a request to reserve the issue. | required |
| AI use disclosed where required | The repo-facts `contribution policy` line (live: `CONTRIBUTING.md`, `AI_POLICY.md` or similar) read against the claim comment and repro report. | Apply the **AI use disclosed where required** table below. Every package in this course is treated as AI-assisted work. | required |
| Control run shown | The report's artifacts. | The report also shows a run that does **not** trigger the bug (the working input, the other config) next to the failing one. | preferred |

### Artifact matches the stated outcome

First read what the report says happened, then check the artifact
against that.

| Report says | Pass when | Fail when |
|---|---|---|
| Reproduced | The artifact shows the issue's symptom itself: the same error text, exit code, wrong output, or crash the issue describes. | The artifact shows something else (a graceful validation error where the issue has a crash, garbled output with the program still running, only that the program starts) and it is described as the issue's behavior. |
| Could not reproduce | The artifact shows the real attempt and its actual result, **and** the report names what may differ from the issue's conditions. | The artifact is missing, or the report gives no idea why its result differs. |

An honest, evidenced cannot-reproduce **passes**. A confident
reproduction of the wrong behavior **fails**.

### AI use disclosed where required

| Contribution policy says | Grade |
|---|---|
| Nothing about AI, or no policy found | pass |
| AI allowed with conditions that do **not** require disclosure (understand it, test it, take responsibility, write comments in your own words) | pass: these are terms to follow, not a disclosure test |
| Disclosure of AI use **is required**, and the claim comment or repro report says AI was used and how | pass |
| Disclosure of AI use **is required**, and neither comment says so | fail |
| AI-assisted contributions are banned | fail |

## Verdict rule

**Accept** (ready to post) only when every applicable `required` check
grades `pass`. Any applicable required check graded `fail` or
`unclear` produces **reject** (hold): proof you cannot verify is not
ready to post.

On a claim-only draft (live mode), the seven repro-report checks grade
`unclear` with evidence `not yet applicable: claim-only draft` and are
left out of the verdict. The verdict then rests on the three claim
checks.

The `preferred` check never changes a verdict. It is reported so a
ready package without a control run can still be improved.

Grade what the package shows, not how it looks. A terse report that
meets every check passes; a long, polished one that misses the trigger
or has no artifact fails. Step count, length, and headings are never
evidence.
