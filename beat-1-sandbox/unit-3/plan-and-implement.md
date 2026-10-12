# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

2D9U

**Plan comment**
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-6120153235

## Plan for #60
I used AI to help draft this plan and ran every step myself.

Following up on [my reproduction](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-6117957890) (macOS arm64, Python 3.11.15, `main` f89c06f).

**Cause:** line 38 of `rag/evaluator/faithfulness_checker.py` uses `chunk.get("text", "")`. The default only applies when the key is missing, so `{"text": None}` returns `None` and `" ".join(...)` raises `TypeError: sequence item 0: expected str instance, NoneType found`. My controls back this up: `[{}]` and `[{'text': 'Knows Python.'}]` both return `0.0` with no exception, so only a present `None` fails.

**Change:** `chunk.get("text") or ""` on line 38, so `None` behaves like a missing key. This is the same line-38 change Akhan521, ShalomDee and others proposed above, and what the open PRs #74 and #154 do. As CONTRIBUTING asks for seeded bugs, I'll delete the #60 `xfail(strict=True)` marker on `test_none_context_chunk_text`. I'll also add one test where a `None` chunk sits next to a real one; it checks the score equals the real chunk alone, so it holds whether or not #59 lands.

**Not in scope:** the real-string control scoring `0.0` instead of `1.0` (that's #59), the `RelevanceScorer` crash on the same input, and any change to claim extraction or scoring.

**How I'll check it:** re-run my repro commands. The `None` case should return `0.0` with no traceback, the two controls should stay `0.0`, and the test should go from `1 failed` (under `--runxfail`) to `1 passed` without the flag. Then `make check && make test-unit`.

**Open question:** `or ""` also maps other falsy values to `""`. bing-ying-li suggested an `is None` check to avoid that; I kept `or ""` because only `None` is reported, I haven't seen a caller pass other falsy values, and it matches #74. If #74 or #154 merges first, I'll rebase and offer just the mixed-chunk test.

---

## Your branch

**Branch**

fix/60-none-chunk-text

**Evidence**

Before (fork at `main`, commit f89c06f, macOS 26.6.2 arm64, Python 3.11.15):

```
$ git rev-parse --short HEAD
f89c06f
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{}])"
2026-10-12 00:37:51 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python.'}])"
2026-10-12 00:37:51 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{'text': None}, {'text': 'Knows Python.'}])"
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: sequence item 0: expected str instance, NoneType found
$ pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text --runxfail -q
rag/evaluator/faithfulness_checker.py:38: TypeError
=========================== short test summary info ============================
FAILED tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text
1 failed in 0.17s
$ pytest tests/unit/test_faithfulness_checker.py -q
..x...x......x....x...                                                   [100%]
18 passed, 4 xfailed in 0.13s
```

After (branch `fix/60-none-chunk-text`, commit 305c66c, same machine):

```
$ git branch --show-current
fix/60-none-chunk-text
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{'text': None}])"
2026-10-12 00:40:36 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{}])"
2026-10-12 00:40:36 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{'text': 'Knows Python.'}])"
2026-10-12 00:40:36 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
$ python -c "...FaithfulnessChecker().check('Knows Python.', [{'text': None}, {'text': 'Knows Python.'}])"
2026-10-12 00:40:36 [info     ] faithfulness_checked           claims_count=1 score=0.0 supported_count=0
0.0
$ pytest tests/unit/test_faithfulness_checker.py::TestFaithfulnessChecker::test_none_context_chunk_text --runxfail -q
1 passed in 0.24s
$ pytest tests/unit/test_faithfulness_checker.py -q
..x...x......x.........                                                  [100%]
20 passed, 3 xfailed in 0.17s
```

Repo checks on the branch: `make check` clean (ruff "All checks passed!", black unchanged, mypy "Success: no issues found in 76 source files"); `make test-unit`: `377 passed, 52 xfailed`.

## Eval iterations

**Run history**

1. Run 1 (full, 20 packages): 19/20 agreement, categories clear-accept 6/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4 (bar: 18/20: PASS). This is the run saved in `eval-run.txt`.

One run only: it cleared the bar and matched every category, so I stopped instead of spending more credit on the one miss. (An earlier launch exited at once with "error: skill not found" because I passed the skill folder instead of `SKILL.md`; nothing was graded.)

**Package analysis**

pkg-14 (zellij-org/zellij#5174). Gold: accept. My rubric: reject, failing only "Cause fits the repro evidence".

The plan blames the reattach path wiring stdin to the session before the OSC 4/10/11 color-query responses are consumed. The grader's evidence line was: "Cache control (clean once after rm -rf ~/.cache/zellij, then leaks) isolates cached color state, not stdin wiring; plan explains it only by an unevidenced 'refetched along the fresh-attach path' and defers the cache". My procedure tells the grader to ask, for each control, "If the plan's cause were true, would this observation look like this?" and that "Any single 'no' is a fail." The plan does explain the cache control ("with an empty cache the color data is refetched along the fresh-attach path once"), but nothing in the repro evidence shows that bridge, so the grader read it as a "no". The gold label accepts it because the main evidence (fresh attach clean, every reattach leaks, 0.44.1 clean) fits the cause, and the plan's explanation of the cache control is plausible even if unproven. My rubric is stricter than gold here on purpose (see the wrong-cause packages under Trade-offs), and this is the cost of that strictness.

**Check rationale**

| One bounded change | The plan's change / scope / approach section, including any "while I'm here", "phase 2", or "also" items. | Everything the plan commits to doing is needed to fix the reproduced behavior: the fix itself, its regression tests, removing a marker or suppression the fix makes stale, and the same fix at sibling call sites on the same code path. Fails when the plan also commits to work the issue did not ask for: refactors or rewrites, dependency upgrades or migrations, new options/settings/UI, new frameworks, ports to other components, CI or harness changes. Work explicitly listed as out of scope or deferred to a follow-up does **not** count against the plan. | required |

Why it reads this way: the first version I considered was shape-based ("the plan has a Not in scope line"), and I rejected it because every scope-creep package could add that line and still bundle a redesign, and terse accepts like pkg-02 might not use the heading. So the pass condition judges the work items themselves: it lists what counts as needed (fix, regression tests, the stale `xfail` marker or suppression, the same fix at sibling sites) and what counts as extra (refactors, upgrades, migrations, new options/UI, frameworks, ports, CI changes). I added the last sentence, that explicitly deferred work does not count, because pkg-09 and pkg-14 are gold accepts that are "honestly scoped-down": they name the bigger rework and defer it. Without that sentence a grader could count the named rework against them. In run 1 the check passed pkg-09 ("option 1 and regex-mode changes are stated deferrals") and pkg-14, and failed every scope-creep package, including pkg-15, where the core fix is right: "Items 2-5 are extra and not deferred: 'replace node-fetch with undici', Settings timeout field, sync-failure UI surfacing, retry-with-backoff wrapper."

**Trade-offs**

This check is the only required check pkg-15 failed in run 1, so it alone decides that package; gold calls pkg-15 "arguable because the core fix is right", and my rubric gives no partial credit for a correct core fix bundled with extras. A plan with a good fix plus one small unasked-for item gets held. What it gives up in the other direction: the "deferred work does not count" clause takes the plan's word for it. A plan that writes "Out of scope: the undici migration (follow-up PR)" and then quietly does it in the build passes here; only the Deviations section and the Unit 4 diff would catch that. It also doesn't judge whether a deferral is wise: pkg-09's deferral is "arguable" per gold, and this check passes it without asking. The "same fix at sibling call sites" allowance could let a plan touch more files than the repro shows; I accepted that because, in calib-01, checking the shared callback from both commit views is exactly the right thing to do.
