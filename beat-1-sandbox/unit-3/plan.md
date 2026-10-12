# Plan: #60 FaithfulnessChecker crashes when a context chunk has `text: None`

Builds on my repro: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60#issuecomment-6117957890
(macOS 26.6.2 arm64, Python 3.11.15, fork at `main` f89c06f).

## Diagnosis

`FaithfulnessChecker.check()` builds the context on line 38 of
`rag/evaluator/faithfulness_checker.py`:

```python
context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
```

The `""` default only applies when the `text` key is missing. With
`{"text": None}` the key is present, `.get()` returns `None`, and
`" ".join(...)` raises before any scoring runs. My repro shows exactly that
split:

```
check('Knows Python.', [{'text': None}])
  File ".../rag/evaluator/faithfulness_checker.py", line 38, in check
    context_text = " ".join([chunk.get("text", "") for chunk in context_chunks])
TypeError: sequence item 0: expected str instance, NoneType found
```

Controls from the same run: `[{}]` (no `text` key) returns `0.0`, and
`[{'text': 'Knows Python.'}]` returns `0.0` with no exception. So a missing
key and a real string both get past line 38; only a present `None` crashes,
which rules out claim extraction and `_is_supported()` as the cause.

## Scope

In scope:
- Line 38 of `rag/evaluator/faithfulness_checker.py`: treat a `None` text the
  same as a missing key.
- `tests/unit/test_faithfulness_checker.py`: delete the issue #60
  `@pytest.mark.xfail(strict=True, ...)` marker on
  `test_none_context_chunk_text` (CONTRIBUTING says a seeded-bug fix must drop
  it, or CI fails with `XPASS(strict)`), and add one test where a `None` chunk
  sits next to a real chunk.

Not in scope:
- Why `[{'text': 'Knows Python.'}]` scores `0.0` instead of `1.0`. That is
  issue #59 (short claims are never marked supported) and its own xfail tests.
- The same `None` crash in `RelevanceScorer` that classmates found
  (different file, different exception, not tracked by #60).
- Any change to claim extraction, scoring, or the `check()` signature.

## Files

- `rag/evaluator/faithfulness_checker.py` (one line)
- `tests/unit/test_faithfulness_checker.py` (remove one marker, add one test)

## Approach

1. Change line 38 to `" ".join([chunk.get("text") or "" for chunk in context_chunks])`.
   `or ""` makes `None` behave exactly like a missing key; real strings are
   unchanged, and `""` was already the missing-key value. I picked this over
   skipping `None` chunks (same score, extra branch) and over raising (breaks
   `test_none_context_chunk_text`, which expects a float).
2. Remove the #60 xfail marker from `test_none_context_chunk_text`. Leave the
   #59 markers alone.
3. Add `test_none_chunk_next_to_real_chunk`: the score for
   `[{"text": None}, {"text": X}]` must equal the score for `[{"text": X}]`.
   It compares the two scores instead of asserting `1.0`, so it stays correct
   whether or not #59 is fixed.
4. Run `make check && make test-unit`.

## Test plan

Re-run my Unit 2 repro commands on the fix branch:

| Command | Before (main f89c06f) | Expected after |
|---|---|---|
| `check('Knows Python.', [{'text': None}])` | `TypeError` at line 38 | returns `0.0`, no traceback |
| `check('Knows Python.', [{}])` (control) | `0.0` | `0.0` (unchanged) |
| `check('Knows Python.', [{'text': 'Knows Python.'}])` (control) | `0.0` | `0.0` (unchanged; #59) |
| `check('Knows Python.', [{'text': None}, {'text': 'Knows Python.'}])` | `TypeError` | same score as the real chunk alone |
| `pytest ...::test_none_context_chunk_text --runxfail` | `1 failed` (TypeError) | `1 passed` |
| `pytest tests/unit/test_faithfulness_checker.py` (no `--runxfail`) | `18 passed, 4 xfailed` | `20 passed, 3 xfailed` (marker gone, new test added; the three #59 xfails stay) |

## Risks and unknowns

- `or ""` also turns other falsy values (`0`, `False`, `[]`) into `""`. I have
  not seen any caller pass those; a non-string truthy value (say an int) would
  still crash the join. I am not handling those because #60 is about `None`.
- Several classmates and the open PRs #74 and #154 propose the same line-38
  change. I reached it from my own repro; if either merges first, I will rebase and offer only the
  mixed-chunk test.

## Deviations

Nothing in the build changed from the plan. The branch `fix/60-none-chunk-text`
(commit 305c66c) has the one-line `or ""` change on line 38, the #60 xfail
marker removed, and the new `test_none_chunk_next_to_real_chunk` test. Every
row of the test plan came out as expected: the `None` case returns `0.0`, both
controls still return `0.0`, the mixed case returns the real chunk's score,
`test_none_context_chunk_text` passes with and without `--runxfail`, and the
file shows `20 passed, 3 xfailed`. `make check` and `make test-unit`
(`377 passed, 52 xfailed`) are clean.

One wording change in comment.md before posting, not in the build: after the
first plan-check run I named bing-ying-li's `is None` suggestion and said why
I kept `or ""`, because my voice guide asks me to answer what the thread
already said. The second plan-check run pointed out the open PR #154, so the
comment now names it next to #74.
