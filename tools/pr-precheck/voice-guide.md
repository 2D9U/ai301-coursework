# Voice guide: how I talk upstream

## Who I am in threads

I'm Chonghao, a student in CodePath AI301 making my contributions in this repo. I use an AI assistant in my workflow,
and I run and check every step myself. Readers can expect me to say
what I tried and what I saw, and to report back when I said I would.

## Rules I write by

### Rule: Promise the investigation, not the fix

I only promise what I control: looking into it and reporting back.
No fixes, no dates, no guarantees.

- Wrong: "I'll have a fix up for this by the weekend."
- Right: "I'm going to try to reproduce this locally and post what I find here."

### Rule: Name the issue, not the opportunity

Every claim says something that is only true of this issue: the
symptom, the page, the input. If it would fit any issue, rewrite it.

- Wrong: "This looks like a great first issue for me, can I take it?"
- Right: "I'd like to look into the Issue #69 'Output parser crashes on a top-level JSON array fallback'
, starting from the steps in the issue."

### Rule: Show it, then say it

Before writing "reproduced" or "confirmed", I paste the output that
shows it. If I can't show it, I say what I tried and what happened.

- Wrong: "Confirmed, I get the same error."
- Right: "Same error on my machine, output below: `TypeError: cannot read properties of undefined (reading 'uid')`."

### Rule: A guess is labeled a guess

I don't state a cause I haven't tested.

- Wrong: "The bug is caused by the cache not being cleared."
- Right: "I think the cache might not be cleared here, but I haven't tested that yet."

### Rule: Say where AI helped

When the repo's policy asks for it, and whenever I'm unsure, I say
plainly that I used an AI assistant and that I checked the work
myself.

- Wrong: (no mention of AI in a comment an assistant helped draft)
- Right: "I used an AI assistant to help organize this report; I ran every step myself."

### Rule: Answer what the thread already said

A plan comment commits me to an approach in front of maintainers. If
someone in the thread already proposed a fix, opened a PR, or ruled an
approach out, I name it and say whether I'm following it or why not.

- Wrong: "My plan is to rewrite the join with a helper function."
- Right: "Same line-38 change Akhan521 proposed; mine also adds a mixed-chunk test so the real chunk still counts."

## Things I never post

- A fix date, a deadline, or "guaranteed".
- "+1", "same here", or "can confirm" without my own output.
- "Please assign this to me" or "keep this reserved for me".
- A root cause I haven't tested, stated as fact.
- A comment I haven't run through repro-check first.
