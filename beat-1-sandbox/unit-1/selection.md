# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60

**Verdict output**

````
Accepted candidates, in fit order:

1. #72 — `verify_password` raises `UnknownHashError` on malformed stored hashes —
   backend auth/service work, the area my Spring Boot, .NET and gRPC experience maps
   onto most directly; the only candidate with a stated effort estimate (1–2 hours)
   and a built-in done-signal (remove the `@pytest.mark.xfail` marker for manifest
   id H-05).
2. #57 — Tech detector counts vendored and build-output files — leans on polyglot
   breadth (knowing `node_modules/` and `build/` are vendored rather than authored
   JS is the whole fix), with a runnable repro and two named failing tests.
3. #60 — Faithfulness checker crashes on `text: None` — closest to the stated
   RAG/evaluation interest, and the smallest change (`chunk.get("text", "")` returns
   `None` when the key exists with a `None` value), but it is a one-line coalesce,
   so it exercises the least.

Rejected: none.

Shared repo facts: `archived: no`, newest default-branch commit 2026-09-16 (4 days
before today's 2026-09-20); no `CONTRIBUTING.md`, `AI_POLICY.md` or `AGENTS.md` in
the repo, so the contribution policy is silent and passes; maintainer first-response
sample shows #52 and #43 answered in 6 days. The Path Review house rule on classmate
claims did not need to apply — all three threads have zero comments and the repo has
zero open PRs.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "Repo is alive", "grade": "pass", "evidence": "archived: no; newest default-branch commit 2026-09-16, 4 days before today (2026-09-20), well within 180"},
      {"name": "Newcomer-sized scope", "grade": "pass", "evidence": "One bounded fix: make verify_password fail closed in core/security.py and drop the xfail marker in tests/unit/test_security.py; two files are parts of one change, not separate PRs"},
      {"name": "Nobody is already on it", "grade": "pass", "evidence": "assignees: none; repo has zero open PRs and search finds 0 PRs referencing #72; 0 comments, so no claim language"},
      {"name": "Contribution policy permits AI-assisted work", "grade": "pass", "evidence": "No CONTRIBUTING.md, .github/CONTRIBUTING.md, AI_POLICY.md or AGENTS.md in the repo (all HTTP 404): silence passes"},
      {"name": "Maintainers answer newcomers", "grade": "pass", "evidence": "Sampled #52 and #43 (opened 2026-09-10) each drew a maintainer first reply on 2026-09-16, 6 days"},
      {"name": "Signposted for newcomers", "grade": "pass", "evidence": "Labels include 'good first issue' (also bug, api, tier-1), applied by COLLABORATOR Aburke225"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57",
    "checks": [
      {"name": "Repo is alive", "grade": "pass", "evidence": "archived: no; newest default-branch commit 2026-09-16, 4 days before today (2026-09-20)"},
      {"name": "Newcomer-sized scope", "grade": "pass", "evidence": "Single bounded defect: exclude node_modules/ and build/ paths in tech_detector.py; runnable repro given and two named failing tests bound the work"},
      {"name": "Nobody is already on it", "grade": "pass", "evidence": "assignees: none; no open PRs in repo and 0 PRs reference #57; 0 comments, no claim language"},
      {"name": "Contribution policy permits AI-assisted work", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI policy file present in the repo: silence passes"},
      {"name": "Maintainers answer newcomers", "grade": "pass", "evidence": "Maintainer first-response sample shows #52 and #43 answered in 6 days, inside the 7-day threshold"},
      {"name": "Signposted for newcomers", "grade": "pass", "evidence": "Labels include 'good first issue' (also bug, agent, tier-1)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "Repo is alive", "grade": "pass", "evidence": "archived: no; newest default-branch commit 2026-09-16, 4 days before today (2026-09-20)"},
      {"name": "Newcomer-sized scope", "grade": "pass", "evidence": "One-line semantics bug in faithfulness_checker.check(); repro given and one named failing test, test_none_context_chunk_text"},
      {"name": "Nobody is already on it", "grade": "pass", "evidence": "assignees: none; no open PRs in repo and 0 PRs reference #60; 0 comments, no claim language"},
      {"name": "Contribution policy permits AI-assisted work", "grade": "pass", "evidence": "No CONTRIBUTING.md or AI policy file present in the repo: silence passes"},
      {"name": "Maintainers answer newcomers", "grade": "pass", "evidence": "Sampled #52 and #43 each drew a maintainer reply 6 days after opening"},
      {"name": "Signposted for newcomers", "grade": "pass", "evidence": "Labels include 'good first issue' (also bug, rag, tier-1)"}
    ],
    "verdict": "accept"
  }
]
```
````

The run graded all three candidates I put to it; the object for
`.../issues/60` records `"verdict": "accept"`.

---

## Eval iterations

**Run history**

Four runs, in order:

1. `4/5` — partial probe, `--only issue-04,issue-09,issue-12,issue-18,issue-20`.
   I probed these five before running anything full because each stresses a
   threshold I was unsure of: issue-04 is a one-line body I needed *not* to fail on
   scope, issue-09 is an accept carrying a stale 2022 claim, issue-12 is the only
   policy item, and issue-18's claim comments are all 300+ days old so its open PRs
   have to do the rejecting. The miss was issue-20, graded accept against a gold
   reject.
2. Rubric edit, no run. My fourth scope disqualifier was "Unspecified feature wish"
   and required that the issue state no acceptance criteria. issue-20's body defeats
   that — it is a filled-in template with a "Success looks like:" line. I rewrote it
   as "Unendorsed feature request", keyed on maintainer signal rather than spec
   quality, and exempted bug fixes and documentation changes.
3. `4/4` — partial probe, `--only issue-20,issue-01,issue-16,issue-09`. issue-20 to
   confirm the fix, issue-01 and issue-16 as canaries, because both are gold-accepts
   with no maintainer comments and no newcomer label — the shape my new rule was most
   likely to start failing.
4. `18/20` — full run, saved with `--save-run`. This is the committed
   `eval-run.txt`, whose agreement line reads
   `agreement: 18/20 scored items  (bar: 18/20: PASS)`, with categories
   `claimed 4/4  clear-accept 6/8  dead-repo 3/3  policy 1/1  scope 4/4`.

Probes cost about $1.80 in total, against roughly $8 for two full runs.

**Issue analysis**

`issue-19` (`zxcalc/zxlive#517`, "Selecting large subgraphs in proof mode freezes the
UI"). **My rubric decided `reject`. The gold label is `accept`.**

My Newcomer-sized scope check failed it on disqualifier (1), umbrella or tracking
issue. The evidence line my run recorded was:

> Body lists 2 potential causes plus 'Additional suggestions': multi-processing for
> parallel matching, matching only expanded categories, and moving rewrite
> application to a separate thread — a set of distinct sub-tasks resembling an
> umbrella issue rather than one bounded fix

That reading follows my own wording, which is why it is worth fixing rather than
shrugging at. The disqualifier says the body must not list "sub-issues or sub-tasks
meant to become separate pull requests", and the bundle's body does enumerate:

> There are two potential causes which should be fixed: 1. The matchers are slow for
> certain rewrites (quadratic instead of linear) 2. UI update is waiting for the
> matching thread to finish

But that is a maintainer diagnosing one symptom and naming its causes — one pull
request someone thought about carefully, not several. My other miss, `issue-01`
(`conda/conda#16475`, also `reject` against a gold `accept`), failed the same way:
its body enumerates the doc files that a single docs change would touch. Both
disagreements are one bug. My check cannot tell *parts of a single change* from
*independent deliverables*.

What actually separates them is who the items are for. `issue-05` ("PRs are welcome
both big and small") and `issue-10` (a literal list of issue numbers) hand different
items to different contributors. `issue-19` and `issue-01` describe the inside of one
contributor's change. The fix is a counter-guard: an enumerated list only counts as an
umbrella when its items ship independently. I did not apply it before submitting,
because the committed run has to stay fingerprint-matched to the `rubric.md` I
uploaded and 18/20 already clears the bar.

**Check rationale**

**Contribution policy permits AI-assisted work**, the fourth required check, quoted as
it is currently written in `tools/issue-select/rubric.md`:

> The policy does **not** ban AI-assisted contributions. Treat as **fail** an
> outright ban ("we do not accept AI-generated code or documentation") and any
> policy that closes AI-assisted pull requests on suspicion alone. Treat as
> **pass**: silence (no `CONTRIBUTING.md`, or no statement on AI — most repos,
> and not a restriction), and **conditions**, which are terms to follow rather
> than reasons to walk away (disclose AI use, personally understand and test
> every change, human-review AI output, "AI-generated PRs that appear untested
> or not understood are closed"). A repo shipping `AGENTS.md` is the opposite
> of a ban.

My first draft had three required checks — liveness, scope, claims — and it would not
merely have scored badly, it would have missed the category floor outright. `policy`
is a one-item category, `issue-12` (`bookwyrm-social/bookwyrm#1133`), and that issue
passes the other three cleanly: the repo pushed the day it was captured, the change is
bounded and labelled `good first issue`, and there is no assignee and no linked PR. The
only thing that rejects it is the repo-facts line:

> "Meaningful human interaction is the whole point of BookWyrm. We do not accept
> AI-generated code or documentation."

Most of the wording goes to the boundary rather than to the ban, because the ban is the
easy half. My other checks catch bad issues; this one mainly has to avoid discarding
good ones. Two of the three repos behind my accepts state an AI policy and both are
permissive — conda's

> generative AI tools welcome; you are responsible for all contributions and must
> review and understand AI-generated content before including it in a pull request

and Zulip's requirement that contributors "personally understand, test, and be able to
explain every change". Those are terms of engagement, not walls. Had I written the
check as "the repo places no restriction on AI use", conda's three accepts would have
failed on a policy that explicitly welcomes me. Hence the three named outcomes: ban
fails, conditions pass, silence passes. Silence is also why this check carries the one
exception to my `unclear`-counts-as-fail verdict rule — a missing `CONTRIBUTING.md` is
a finding, not an evidence gap.

**Trade-offs**

What this check gives up.

**It changed exactly one result, and that was the point.** `issue-12` went from accept
to reject, taking the `policy` category from 0/1 to 1/1. Nothing else in the set moved,
and I can say why: only four of the twenty bundles state any AI policy at all. conda
(issues 01, 09, 16) and Zulip (08, 15) state permissive ones, which pass as conditions,
so those five verdicts are untouched. tldr-pages (`issue-10`) states

> strongly discourages generative AI for new pages (output is often inaccurate); pull
> requests suspected of being made wholly or partly with generative AI or machine
> translation without human review are closed

which my check reads as a ban — but `issue-10` was already rejected as a megaissue on
scope, so the policy check earns nothing there. The remaining fifteen bundles say
nothing about AI and pass on silence.

**The case I accept it will miss:** it can only read a *stated* policy. A repo with no
`CONTRIBUTING.md` whose maintainers quietly close AI-assisted PRs passes this check on
silence, and I would find out only after opening the PR. The check is also weakest
exactly where judgement is needed most — tldr-pages sits between "condition" and "ban",
and my wording resolves it by the consequence ("closed") rather than by the verb
("discourages"). A repo that discourages AI without saying what it does about it would
pass, which I am not confident is right.

---

## Selection rationale

**Selection rationale**

**1. Fit to my interests and the time available.** My background is backend and systems
work — HTTP and gRPC services, auth, data layers, tests. What I want more practice in is
the AI side of a production system rather than the plumbing I already know. `#60` is in
`rag/evaluator/`, the code that scores model output, which is squarely that. It is also
small: the bug is that `chunk.get("text", "")` returns `None` when the key exists with a
`None` value, so `" ".join(...)` raises `TypeError`. One named test,
`test_none_context_chunk_text`, tells me when I am done. For a first contribution where
most of the work is learning the claim-and-PR workflow rather than the fix itself, I
would rather the fix be small and the target unambiguous.

**2. What the verdict got right, and what I weighed that it could not.** The rubric was
right on everything mechanical: the repo is four days warm, nobody holds the issue, no
policy blocks me, and it carries a maintainer-applied `good first issue` label. What it
could not weigh is that it ranked `#60` *third*. My fit profile drove it to prefer `#72`
(auth, the work closest to what I have already done) and `#57` over it. I overrode that
deliberately — fit ordering is advisory, and the profile optimises for what I am already
good at, while I picked the issue that moves me toward what I am not. The rubric also
cannot see that `#60`'s one-line fix leaves me room to spend Unit 2's effort on the
process rather than the code, which for a first PR is the trade I want.

**3. Anticipated difficulty in claiming it.** Low, with one caveat. The issue has no
assignee, no linked PR, and zero comments, and the repo has no open PRs at all, so
nothing is contested today. The caveat is that `#60` is a `tier-1` `good first issue` in
a classroom repo of 71 open issues where every one of my classmates is choosing this
week from the same shortlist, and the easiest tier-1 bugs are the obvious picks. I
expect other claim comments to appear on it. Per the Path Review house rule in my
`scope.md`, that does not block me — credit attaches to the PR I open, not to whether it
merges — so the real risk is duplicated effort rather than a lost issue. I have not
commented yet; choosing is not claiming, and the claim comment comes in Unit 2.

---
