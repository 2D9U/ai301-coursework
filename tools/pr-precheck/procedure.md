# Procedure: how this tool grades a PR package

Four stages, run in order. Do not grade any check before stage 3.

## Read order

Read the plan before the PR, so the diff and description are compared
against what was promised instead of against what they say about
themselves.

1. **Plan context first** (live: `plan.md`). Write down: the files the
   plan names; every change it commits to (including tests, docs,
   changelog); everything it lists as out of scope or deferred; every
   deviation note; the test plan's failure cases (list each one
   separately if it names more than one) and the expected result for
   each; and the repro's exact trigger (command, input, flags).
2. **Repo facts** (live: `.github/PULL_REQUEST_TEMPLATE.md`,
   `docs/CONTRIBUTING.md`). Copy the `pull requests` line and the
   `contribution policy` line word for word. List each stated PR ask
   (template sections, checklist items, issue reference, changelog or
   whatsnew entry, test/check commands) and whether AI disclosure is
   required. In live mode, add Path Review's house disclosure ask.
3. **Issue and thread highlights**. Note the symptom and any maintainer
   signal (OWNER / MEMBER / COLLABORATOR / CONTRIBUTOR direction,
   question, or open PR).
4. **Diff** (live: `git diff main...HEAD`). For each hunk, write one
   line: file, what it changes, and whether it is the fix, a test, docs,
   or something else.
5. **Commits** (live: `git log --oneline main..HEAD`). List messages.
6. **Description** (live: `pr_draft.md`). List every claim about what
   the PR does or does not do, every deviation or deferral it states,
   each template section and its content, and any AI disclosure.
7. **Test evidence** last (live: the description's Testing section and
   `test_evidence.md`). For each run, note the command, its input, and
   the printed before and after.

## Evidence gathering

Build these side-by-side pairs from the notes above. Each check reads
exactly one pair.

1. **Diff stays inside the plan**: hunk list (read step 4) next to the
   plan's files, commitments, out-of-scope list, and deviation notes
   (read step 1). Mark each hunk "planned", "deviation-noted", or
   "unplanned". Then mark each plan commitment "in diff", "deferred
   with a note", or "missing".
2. **Description matches the diff**: description claims (read step 6)
   next to the hunk list (read step 4).
3. **Plan's repro re-run before and after**: test plan cases and repro
   trigger (read step 1) next to the evidence runs (read step 7).
   For each case, record: re-run with the same trigger? before shown?
   after shown and matches the expected result?
4. **Repo's checks run with results**: stated test/check commands
   (read step 2) next to the evidence runs (read step 7).
5. **No debris in the diff**: the added and changed lines of every
   hunk, scanned for the debris list in the rubric, plus the commits.
6. **Repo's stated PR asks met**: the ask list (read step 2) next to
   the description sections (read step 6) and the hunk list.
7. **AI use disclosed where required**: the copied policy line (read
   step 2) next to the description's disclosure (read step 6).
8. **Commits are reviewable**: the commit list (read step 5).
9. **Description engages the thread**: maintainer signals (read step 3)
   next to the description.

## Check execution

1. Run the checks in rubric order, one at a time, using only the pair
   gathered for that check. One check's result never changes another's.
2. **Diff stays inside the plan**: any "unplanned" hunk is a fail; any
   "missing" commitment is a fail. A file the plan never names is
   "unplanned" even if the description says otherwise.
3. **Description matches the diff**: test each claim against the hunk
   list. One false claim is a fail. A claim that something was not done
   ("no options added") is true when no hunk does it.
4. **Plan's repro re-run before and after**: every test plan case must
   have all three marks "yes". A run of a different input than the
   repro's trigger does not count for that case. A sentence with no
   command and no observed result does not count as a run.
5. **Repo's checks run with results**: pass if at least one stated
   suite/check command is named with an outcome; an honest failing
   result with a reason still passes.
6. **No debris in the diff**: one debris line is a fail; quote it.
7. **Repo's stated PR asks met**: check each listed ask; one visibly
   skipped ask is a fail; quote the ask. No stated asks is a pass.
8. **AI use disclosed where required**: find the matching row of the
   rubric's table and take its grade.
9. Grade `pass` or `fail` whenever the package holds the evidence.
   Grade `unclear` only when the part the check reads is absent from
   the package entirely; a PR that omitted evidence it owed is `fail`.
10. Each grade gets a one-line evidence quote or fact.

## Verdict assembly

1. Collect the seven `required` grades.
2. All seven `pass` gives `accept`. Any `fail` or `unclear` gives
   `reject` (`unclear` counts as `fail`).
3. The two `preferred` grades never change the verdict; report them.
4. For a reject, the deciding check is the **first failing required
   check in rubric order**; name it and quote its evidence in the
   summary, then list any other failing required checks.
5. Emit the JSON block last, with all nine checks in rubric order.
