# Procedure: how this skill grades a plan package

Four stages, run in order. Do not grade any check before stage 3.

## Read order

1. Read the **repro-evidence block first**, before the plan. Write down,
   as a short list: the failing step and its artifact, every control
   run and what it showed, any intermediate observation (a value
   printed mid-pipeline, a timing, a `--debug` trace), and the stated
   Expected result. Reading the evidence before the plan keeps the
   plan's confident wording from deciding what the evidence "means".
2. Read the **issue** (title, body, reported symptom) and note the
   symptom the repro must match.
3. Read the **thread highlights**. For each comment, note the author's
   role in parentheses. List every maintainer signal (OWNER, MEMBER,
   COLLABORATOR, CONTRIBUTOR): culprit located, approach proposed or
   rejected, patch/test binary posted, testing asked for, open PR or
   prior art named. If there are none, write "no maintainer signal".
4. Read the **repo-facts** block. Copy the `contribution policy` line
   word for word, and note whether it requires AI disclosure, and if
   so where (comments/issues, all usage, or only the pull request).
5. Read the **candidate plan** and note: its stated cause, every item
   it commits to doing (including "also", "while I'm here", "phase 2"),
   everything it lists as out of scope or deferred, the files or code
   sites named, the approach chosen, the test plan, and its risks.
6. Read the **candidate plan comment** last and note what it says
   about the thread's signals, what it says about AI, and any date or
   outcome it promises.

In live mode, steps 1-4 come from the student's posted repro comment,
the live issue thread, and the repo's `CONTRIBUTING.md` / AI policy
files (see the evidence guide); steps 5-6 come from `plan.md` and the
draft comment only.

## Evidence gathering

For each check, pull exactly this from the notes above (re-open the
package section only to quote it):

1. **Cause fits the repro evidence**: the plan's cause sentence, plus
   every control and intermediate observation from read step 1.
2. **One bounded change**: the list of committed work items and the
   out-of-scope/deferred list from read step 5.
3. **A stranger could start**: the named files/code sites and the
   chosen approach from read step 5; quote any hedge ("somewhere",
   "whichever", "not sure", "maybe also").
4. **Test plan observes the fix**: the test plan sentence(s) and the
   repro's Expected result from read step 1.
5. **Comment follows the thread's direction**: the maintainer-signal
   list from read step 3, the plan's approach, and the comment's words
   about those signals.
6. **AI use disclosed where required**: the copied policy line from
   read step 4 and any AI mention in the comment from read step 6.
7. **Unknowns stated as unknowns**: the plan's risks section and any
   certainty claim about untested behavior.
8. **Comment matches the plan**: the comment's cause/change/scope next
   to the plan's.

## Check execution

1. Run the checks in rubric order, one at a time. For each, apply the
   pass condition (or its decision table) to the gathered evidence
   only. Do not let one check's result change another's.
2. For **Cause fits the repro evidence**, go through each control and
   intermediate observation and ask: "If the plan's cause were true,
   would this observation look like this?" Any single "no" is a fail.
   Agreement with the issue or the thread is not a pass on its own.
3. For **One bounded change**, mark each committed work item
   "needed for the fix" or "extra". One "extra" item is a fail, even if
   the core fix is correct. Deferred/out-of-scope items are skipped.
4. For **A stranger could start**, ask: "Could someone else open the
   named file and begin today?" A hedge on where or how is a fail; a
   named open question with a stated fallback is a pass.
5. For **Test plan observes the fix**, ask: "Is there a specific
   result someone could see change from failing to passing?" Name it
   in the evidence line.
6. For the two decision-table checks, find the row that matches and
   take its grade. Quote the row's trigger in the evidence line.
7. Grade `pass` or `fail` whenever the package contains the evidence.
   Grade `unclear` only when the evidence the check names is absent
   from the package entirely (for example, no repro block, no test
   plan section and no test sentence anywhere). Missing evidence the
   plan was responsible for providing (no files, no test outcome) is a
   `fail`, not `unclear`.
8. Each grade gets a one-line evidence quote or fact from the package.

## Verdict assembly

1. Collect the six `required` grades.
2. If all six are `pass`, the verdict is `accept`. If any is `fail` or
   `unclear`, the verdict is `reject`. `unclear` counts as `fail`.
3. Ignore the two `preferred` checks for the verdict; still report them.
4. In the readable summary, name the deciding check(s) for a reject
   (every required check that did not pass) with its evidence quote.
5. Emit the JSON block from SKILL.md last, with all eight checks.
