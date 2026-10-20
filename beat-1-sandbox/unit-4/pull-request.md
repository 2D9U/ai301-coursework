# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/231

**Branch**

fix/60-none-chunk-text

## Eval iterations

**Run history**

1. Run 1 (full, 20 packages): 18/20 agreement, categories clear-accept 5/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3 (bar: 18/20: PASS). This is the run saved in `eval-run.txt`.

One full run only. It met the bar with every category matched, and the two misses (pkg-02 and pkg-05) are both cases where I chose to keep a check strict, so I wrote them up below instead of loosening the checks and paying for another full run.

**Package analysis**

pkg-02 (Textualize/rich#4208). Gold: accept. My rubric: reject, failing only "No debris in the diff".

The fix is right and matches the plan: the `save_*` methods export with `clear=False`, write the file, and clear only after the write succeeds. The grader failed it with: "save_html/save_svg hunks remove and re-add identical lines ('-        html = self.export_html(' / '+        html = self.export_html(', 'theme=theme,', 'svg = self.export_svg(', 'title=title,')." It read it that way because my debris check lists "identical lines removed and re-added" as debris. I put that in for pkg-18, whose gold note calls "two blocks of identical re-printed lines (pure churn)" debris. In pkg-02, though, the re-printed lines are the context around the one real change (`clear=clear` to `clear=False`) inside the same call. A human reviewer would read that as a noisy diff of a one-word edit, not as unrelated churn. My check can't tell "re-printed lines with no change nearby" (pkg-18) from "re-printed lines wrapped around a real edit" (pkg-02). To fix it, the check would need to say that re-printed lines count as churn only when the hunk changes nothing else.

**Check rationale**

| Plan's repro re-run before and after | The test evidence section (live: the description and `test_evidence.md`), read against the plan's test plan and the repro evidence's steps. | Every failure the plan's test plan names is re-run with the **same trigger** as the repro, and the evidence shows its observable result before and after (an exit code, output, error text, value, or byte-for-byte diff), with the after matching the plan's expected result. Fails when the evidence is a claim without an outcome ("tested locally", "works now", "verified for a day"), when it exercises a different input or path than the repro (the control, the unchanged path, a single-file case when the bug needs two), or when it re-runs only some of the failures the test plan names. | required |

Why it reads this way: I started from my Unit 3 "Test plan observes the fix" check, which only asked whether the plan named an observable outcome. For a PR, the question is whether the evidence actually shows that outcome, so I rewrote it around three things the activity's calib-03 and calib-04 taught: the **same trigger** as the repro (pkg-14 runs a GET with header casing when the bug is a single-header POST; pkg-07 runs only the one-file control when the bug needs two files), an observable **before and after** (pkg-04's "colors work now in paging mode" and pkg-10's "verified working on my machine for a full day" are claims, not output), and **every** failure the test plan names (calib-04 re-runs only the first of two crashes). I rejected a looser version, "the evidence includes a before/after run", because pkg-07 and pkg-14 both include runs and would pass it while never touching the fixed path. In run 1 this check was the only failing required check for pkg-07 ("Only the one-file control (./onefile/) was run; the two-file a.go/b.go repro is not re-run and no before/after is shown").

**Trade-offs**

The "every failure the test plan names" clause costs me pkg-05, a gold accept. Its plan's test plan has two cases ("re-run the issue's script, expect both `atuin` rows; re-run with two same-name same-key bindings, expect one row plus a warning"), and the evidence shows before/after tables for the first but only the sentence "Same-key redefine prints the one-time warning" for the second. My grader failed it: "the plan's second case (same-name same-key: one row plus warning) has only the bare claim ... with no command or output." I accept that miss. The same clause is what holds calib-04, whose evidence re-runs only the first of the two crashes its plan names (the staff answer rejects it on exactly that), and I would rather a PR of mine get held for a missing second case than have the tool wave through an untested half of a fix. If I loosened it to "the primary repro case", I would re-run pkg-05 with `--only pkg-05,pkg-07,pkg-14 --include-calibration` and check that pkg-07, pkg-14 and calib-04 still reject, since those three depend on this check.
