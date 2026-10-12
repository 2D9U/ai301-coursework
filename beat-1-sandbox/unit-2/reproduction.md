# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

**GitHub username:* 2D9U

**Issue:** https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60

---

## Claim comment

**Link** https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-6117007275

<!-- TODO: link to your posted claim comment (click the comment's timestamp, copy the URL) -->

Hi, I'd like to investigate the FaithfulnessChecker.check() crash when a context chunk is {'text': None}. My plan is to set up my fork at main (f89c06f) following docs/SETUP.md on macOS (arm64) and run the issue's example. I'll also run test_none_context_chunk_text, with a chunk that has no text key as a control. I'll post my environment, exact commands, and the output here, whether or not it reproduces.

---

## Repro comment

**Link** https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-6117957890

<!-- TODO: link to your posted repro comment -->

## Reproduction of #60
Reproduced myself and Ran with AI assistance
**Environment**
- macOS 26.6.2 (ARM), Python 3.11.15
- my fork at `main`, commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`
- Installed with `pip install -e ".[dev]"` in a Python 3.11 venv (structlog 26.1.0, pytest 9.1.1).
  Skipped Docker, migrations, and the frontend from docs/SETUP.md: `check()` doesn't use them.

**Steps**
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker; print(FaithfulnessChecker().check('Knows Python.', [{'text': None}]))"
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker as F; print(F().check('Knows Python.', [{}]))"
python -c "from rag.evaluator.faithfulness_checker import FaithfulnessChecker as F; print(F().check('Knows Python.', [{'text': 'Knows Python.'}]))"
pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text -v --runxfail

**Observed**
```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/hardwire/Desktop/ai301/pathreview-ai301-fa26-s1/rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
```

**Control**
- `[{}]` (no `text` key) → [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
- `[{'text': 'Knows Python.'}]` → [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0

**Expected:** `check()` returns a float, as `test_none_context_chunk_text` asserts.
**Actual:** It returns a TypeError at rag/evaluator/faithfulness_checker.py:38:
with short test summary info:
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text - TypeError: sequence item 0: expected str instance, NoneType found
```
context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
E       TypeError: sequence item 0: expected str instance, NoneType found
```

## Eval iterations

**Run history**

Four steps, in order:

1. `6/7`: partial probe,
   `--only pkg-03,pkg-05,pkg-07,pkg-09,pkg-11,pkg-16,pkg-20`. I picked these
   because each one tests a call I was unsure of. pkg-05, pkg-07, pkg-03 and
   pkg-20 are the four packages with a stated AI policy (permissive, conditional,
   human-voice, must-disclose). pkg-09 is an honest cannot-reproduce that has to
   pass. pkg-11 is a terse report that has to pass. pkg-16 is a silent
   version deviation that has to fail. The miss was pkg-03, graded reject against
   a gold accept, failing on Claims backed by the artifacts.
2. Rubric edit, then `4/4`: partial probe, `--only pkg-03,pkg-13,pkg-15,pkg-17`.
   pkg-03's report describes a control run in prose ("Dropping `-r '$1'` from
   the same command reports 1, 4, 7, 10 correctly") without pasting its output,
   and the grader read that as an unbacked claim. I added one sentence to the
   check: a plain report of a variant run, naming its exact change and result, is
   an observation, not an unbacked claim. Because that loosens the check, I
   re-ran pkg-03 alongside three packages that need it to stay strict: pkg-13
   ("guaranteed reproducible" with nothing shown), pkg-15 (a root cause asserted
   with no transcript) and pkg-17 (generalizing the bug to a release maintainers
   could not reproduce on). All three still rejected; pkg-03 now accepts.
3. Rubric edit, no run. I rewrote the AI-disclosure decision table from separate
   pass/fail columns into one row per combination of policy and comment, each
   with a single grade, so each row can be checked on its own. The logic did not
   change, so I let the confirming full run cover it.
4. `19/20`: full run, saved with `--save-run`. This is the committed
   `eval-run.txt`, whose agreement line reads
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with categories
   `clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

Probes cost about $2.20 in total (11 package gradings), against about $4 for the
one full run.

**Package analysis**

`pkg-05` (`conda/conda`, `conda env update --json` prints the
`EnvironmentSectionNotValid` warning to stdout and breaks the JSON). **My rubric
decided `reject`. The gold label is `accept`.**

The required check that failed it was Steps re-runnable by a stranger. The
evidence line my run recorded was:

> env.yml contents are described ("a valid dependencies: list plus a category:
> section") but never shown, and it is not a public file

That follows my own wording. The check passes only when "every input a step
needs is shown inline or is public", and the report never prints `env.yml`. But
look at what the report does say:

> wrote a minimal `env.yml` containing a valid `dependencies:` list plus a
> `category:` section (the section conda does not recognize)

That is enough to rebuild the file. Any valid dependency list works, and the
only part that matters for the bug is the unrecognized `category:` key, which is
named. Everything else in the package passed: the environment matches the
issue's conda 26.7.0 / libmamba / osx-arm64 exactly, the command is the issue's
own, and the artifact shows the warning text ahead of the JSON plus
`json.tool` failing on it. The gold label reads the description as followable.
My rubric reads "not shown" as "not re-runnable".

The two cases my check must keep apart are pkg-05 and pkg-18. pkg-18 fails on
the same check for "our company monorepo (private; I cannot share it...)" and
"our internal .golangci.yml... not shareable". There the missing input is
something only the author has. In pkg-05 the missing input is something anyone
can recreate from one sentence. My check treats both as "not shown". The fix
would be to pass an input that is described precisely enough to recreate,
and fail one that is private or unshared. I did not apply it before submitting:
the committed run has to stay fingerprint-matched to the `rubric.md` I uploaded,
19/20 already clears the bar, and loosening this check is exactly the kind of
change that could flip pkg-18 or pkg-06, the two other packages it guards.

**Check rationale**

**AI use disclosed where required**, quoted as it currently reads in
`tools/repro-check/rubric.md`. The row:

> | AI use disclosed where required | The repo-facts `contribution policy` line
> (live: `CONTRIBUTING.md`, `AI_POLICY.md` or similar) read against the claim
> comment and repro report. | Apply the **AI use disclosed where required**
> table below. Every package in this course is treated as AI-assisted work. |
> required |

and its decision table:

> | Contribution policy says | Grade |
> |---|---|
> | Nothing about AI, or no policy found | pass |
> | AI allowed with conditions that do **not** require disclosure (understand
> it, test it, take responsibility, write comments in your own words) | pass:
> these are terms to follow, not a disclosure test |
> | Disclosure of AI use **is required**, and the claim comment or repro report
> says AI was used and how | pass |
> | Disclosure of AI use **is required**, and neither comment says so | fail |
> | AI-assisted contributions are banned | fail |

Without this check the rubric would miss the category floor outright.
`disclosure` is a one-package category, `pkg-20` (`ghostty-org/ghostty`), and
pkg-20 passes every proof check: the environment is recorded, the trigger is the
issue's own, and the artifact shows `997;2` against a `997;1` control. The only
thing that holds it is the repo-facts line:

> All AI usage in any form must be disclosed, stating the tool used and the
> extent of the assistance

and neither comment mentions AI.

I wrote it as a decision table, not prose, because of my Unit 1 feedback: my
"Unendorsed feature request" disqualifier stacked author association, labels,
maintainer comments and two exemptions into one sentence, and nobody could
quickly tell which combination rejected an issue. Here each row is one
combination of two facts, what the policy demands and what the comments say,
with one grade. When a package fails, the row it landed in is the explanation.

Most rows guard the boundary, not the wall. Three other packages state AI
policies, and all three must pass. conda (pkg-05) says "you are responsible for
all contributions and must review and understand AI-generated content", which is
a condition, not a disclosure requirement, so its silent comments pass on row 2.
p5.js (pkg-07) discloses, so it passes either way. ripgrep (pkg-03) requires
comments "written by humans in their own words", which is again a condition. A
looser check ("the repo mentions AI, so the comment must disclose") would have
failed pkg-05 and pkg-03 on policies that welcome AI-assisted work.

**Trade-offs**

What the rubric gives up by grading every check on its own and rejecting on any
failed required check.

**Strictness costs a good package now and then.** pkg-05 is the price of a
strict "show every input" rule. The same strictness is what rejects pkg-18,
where the reproduction lives in a private monorepo. I chose to miss a precisely
described minimal input rather than risk accepting an unreproducible one, since
posting an unreproducible repro wastes a maintainer's time and holding a good
one costs me only a rewrite.

**The disclosure check only reads a stated policy.** A repo that expects
disclosure but never writes it down passes on silence, and I would find out only
after a maintainer asked. In live mode on Path Review this does not bite (the
repo has no `CONTRIBUTING.md` or AI policy file), which is why my voice guide
has me disclose AI help anyway.

**Separate checks make the output longer.** Eleven checks per package is more
to read than a five-family rubric. The payoff is that every rejection names one
specific thing to fix, which is what made the pkg-03 and pkg-05 misses traceable
to a single sentence each.
