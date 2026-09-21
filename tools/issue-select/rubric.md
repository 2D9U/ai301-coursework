# Rubric: is this a good first issue?

Five checks. Four are `required` and gate the verdict; two are
`preferred` and only rank the issues that are accepted.

Every recency threshold below is measured against the **capture date**
stamped at the top of the bundle in eval mode, and against **today** in
live mode. Never against the date you happen to be reading.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Repo is alive | Repo facts: the `archived:` flag on the repo line, the dated `last 5 default-branch commits` list, and `last push to any branch`. | `archived: no`, **and** the most recent commit in the list is dated within **180 days** of the capture date. Judge by the newest date in the list, not by list order — the commits are not always sorted. A bot-authored commit counts only when it is merging a human's pull request (`Merge pull request #N from ...`); a lone `dependabot`/`pre-commit-ci` bump does not. Release recency and star count are **not** part of this check: a repo with no published releases still passes on commits alone. | required |
| Newcomer-sized scope | The issue title, body, labels, author `author_association`, open date, and the full Comments section. | Passes unless **any** of these five disqualifiers holds: (1) **Umbrella or tracking issue** — the body lists sub-issues or sub-tasks meant to become separate pull requests, calls itself a megaissue/tracking issue, or asks for open-ended incremental work spread across the codebase ("add more X to the codebase", "PRs welcome big and small"); (2) **Unsettled after a long debate** — the thread has **20 or more comments** and the issue has been open **more than 2 years** as of the capture date, with no maintainer comment stating a settled approach; (3) **Support request** — the issue asks how to use the software rather than asking for a change; (4) **Unendorsed feature request** — the issue asks for a **new feature or enhancement** (not a bug fix, and not a documentation change: those two are exempt from this disqualifier) and carries **no maintainer signal at all**, meaning all three of: its author's `author_association` is not OWNER/MEMBER/COLLABORATOR, it has no maintainer-applied `good first issue` / `help wanted` / `easy` label, and no maintainer has commented endorsing that it should be built. Building a feature no maintainer has agreed to is a product decision, and a newcomer is the wrong person to make it. **A tidy body is not endorsement**: a filled-in issue template, a “success looks like” line, or a named target file is the reporter's own framing and carries no maintainer weight — and an unresolved dependency (“asset TBD”, “design pending”) confirms the decision is still open. Weigh who backed the issue, not how neatly it was written; (5) **Core-internals warning** — a maintainer says in the thread that the fix requires changes to core internals. **A short body is not a disqualifier.** A one-line bug report, a bare acceptance-criteria checklist, or a bug filed with no reproduction steps is bounded work when the ask itself is bounded. Grade the size of the work requested, not the polish of the writing. | required |
| Nobody is already on it | Repo facts: `this issue: assignees:` and `linked PRs:` with each PR's state. Comments section: claim language ("I'll take this", "working on this", "can I pick this up"), any pull request mentioned in the thread, and the date of each comment. | **All three** hold: (a) the assignee list is empty; (b) no linked or thread-mentioned pull request is in state **open** — a **closed** or **merged** PR is an abandoned or finished attempt, not a claim; (c) no **live** claim comment, meaning a claim dated within **180 days** of the capture date. A claim older than 180 days with no open PR behind it is **stale and does not block** — especially where a maintainer has since invited others to try, or a stale-bot has cycled the issue. | required |
| Contribution policy permits AI-assisted work | Repo facts: the `contribution policy` line (sourced from `CONTRIBUTING.md`, any docs it links out to, or a dedicated AI policy file). | The policy does **not** ban AI-assisted contributions. Treat as **fail** an outright ban ("we do not accept AI-generated code or documentation") and any policy that closes AI-assisted pull requests on suspicion alone. Treat as **pass**: silence (no `CONTRIBUTING.md`, or no statement on AI — most repos, and not a restriction), and **conditions**, which are terms to follow rather than reasons to walk away (disclose AI use, personally understand and test every change, human-review AI output, "AI-generated PRs that appear untested or not understood are closed"). A repo shipping `AGENTS.md` is the opposite of a ban. | required |
| Maintainers answer newcomers | Repo facts: `maintainer first-response sample`; plus any OWNER/MEMBER/COLLABORATOR comment in this issue's thread. | At least one sampled issue drew a maintainer first response within **7 days**, or a maintainer has commented in this thread. Entries reading "no maintainer comment in thread" are common even in healthy repos and never sink an issue — this check only ranks. | preferred |
| Signposted for newcomers | The issue's labels, and who applied them. | The issue carries a maintainer-applied `good first issue`, `help wanted`, `easy`, or `documentation` label. | preferred |

## Verdict rule

**Accept** only if all four `required` checks grade `pass`. Any required
check graded `fail` **or** `unclear` produces **reject**: a first issue
whose evidence you cannot verify is not a first issue you should take.

The two `preferred` checks never change a verdict. Grade and report them
anyway; on an accepted issue they are the reasons to rank it above
another accepted issue.

One exception to treating `unclear` as fail, because silence there is
real evidence rather than missing evidence: on **Contribution policy**,
a bundle or repo that states no policy at all grades `pass`, not
`unclear`. Reserve `unclear` on that check for a policy you found but
genuinely cannot read as ban or condition.
