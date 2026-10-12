# Rubric: is this plan ready to post and build from?

Eight checks, one question each. Six are `required` and gate the
verdict; two are `preferred` and never change it. Each check reads
**one** part of the package against **one** other part, so a failed
check names exactly what to fix. Where a check has to branch, the
branch is a small decision table below the checks.

Four checks read the **candidate plan** (cause, scope, executability,
test plan). Two read the **candidate plan comment** against the
**thread highlights** and the **repo-facts** block. Length, headings,
polish, and confidence are never evidence: a terse plan can pass every
check and a long, confident one can fail.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Cause fits the repro evidence | The plan's stated cause (Diagnosis / Cause / Root cause) read against **every** step, control run, and artifact in the repro-evidence block. | The cause explains the failing run **and** is consistent with every control and every intermediate observation in the repro evidence. Fails when any repro step or control contradicts it: a control where the blamed component works fine, an artifact showing the failure already happened before the blamed step runs, or a timing/variant run that isolates a different component. A cause adopted from the issue or the thread still fails if the repro evidence contradicts it. | required |
| One bounded change | The plan's change / scope / approach section, including any "while I'm here", "phase 2", or "also" items. | Everything the plan commits to doing is needed to fix the reproduced behavior: the fix itself, its regression tests, removing a marker or suppression the fix makes stale, and the same fix at sibling call sites on the same code path. Fails when the plan also commits to work the issue did not ask for: refactors or rewrites, dependency upgrades or migrations, new options/settings/UI, new frameworks, ports to other components, CI or harness changes. Work explicitly listed as out of scope or deferred to a follow-up does **not** count against the plan. | required |
| A stranger could start | The plan's files / areas, approach, and order of work. | The plan names where the change goes (a file, function, or code site) **and** commits to one approach, so someone else could begin without asking the author anything. Fails when a real decision is left to build time: no file or area named, "somewhere", "whichever is easier", "not sure which layer", "profile and see", or a list of options with none chosen. A named, bounded open question with a stated fallback ("if the benchmark shows a cost, move the check to the two call sites") passes. | required |
| Test plan observes the fix | The plan's test plan read against the repro evidence's steps and its stated Expected result. | The test plan names at least one observable, fix-specific outcome tied to the reproduced failure: the repro step re-run with its expected result (an exit code, output, color, value), or a named regression test of the issue's case that must pass. Fails when the only check is a feeling ("should feel fast", "nothing else should break") or a generic run ("run the full test suite", "CI passes") with no outcome for the fix itself. | required |
| Comment follows the thread's direction | The plan comment (and the plan's approach) read against the thread highlights. | Apply the **Comment follows the thread's direction** table below. | required |
| AI use disclosed where required | The repo-facts `contribution policy` line read against the plan comment. Every package in this course is treated as AI-assisted work. | Apply the **AI use disclosed where required** table below. | required |
| Unknowns stated as unknowns | The plan's risks / unknowns section and any sentence asserting certainty about untested behavior. | The plan names at least one real risk or open question, or its claims are all backed by the repro evidence. Fails when untested behavior is stated as certain ("this fixes every case", "no risk"). | preferred |
| Comment matches the plan | The plan comment read against the candidate plan. | The comment describes the same cause, change, and scope as the plan, and promises no fix date or outcome the plan does not support. | preferred |

### Comment follows the thread's direction

First list every **maintainer signal** in the thread highlights: a
comment by an OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR (or a
maintainer named in the repo facts) that locates the culprit, proposes
or rejects an approach, posts a patch or test binary, asks for
testing, or points at an open PR or prior art.

| Thread has | Pass when | Fail when |
|---|---|---|
| No maintainer signal (or no comments) | always | never |
| A maintainer signal | The plan follows it, **or** the comment names it and explains why the plan differs. Engaging an open PR (offering to test or add to it instead of racing it) counts as following it. | The plan goes another way and the comment never mentions the signal: e.g. a docs-only workaround when the owner already located the code culprit and posted a patch, or the exact approach the maintainer rejected. |

### AI use disclosed where required

Read **who** and **where** the policy asks for disclosure. Only a
requirement that covers issue comments (or "all AI usage in any form")
can fail a plan comment; a requirement placed on the pull request is
for the PR, which comes later.

| Contribution policy says | Grade |
|---|---|
| Nothing about AI, or no policy found | pass |
| AI allowed with conditions that do **not** require disclosure in comments (understand the work, test it, take responsibility, write comments in your own words, disclose in the PR) | pass: terms to follow, not a comment disclosure test |
| Disclosure of all AI usage (or AI use in comments/issues) **is required**, and the plan comment says AI was used and how | pass |
| Disclosure of all AI usage (or AI use in comments/issues) **is required**, and the plan comment says nothing about AI | fail |
| AI-assisted contributions are banned | fail |

## Verdict rule

**Accept** (ready to post and build from) only when all six `required`
checks grade `pass`. Any `required` check graded `fail` **or**
`unclear` produces **reject** (hold): a plan you cannot verify from
the package is not ready to build from. `unclear` is only for evidence
that is genuinely absent from the package (for example, no repro
evidence block at all); it is never a softer fail.

The two `preferred` checks never change the verdict. They are reported
so a ready plan can still be improved.
