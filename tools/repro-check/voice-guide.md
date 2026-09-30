# Voice guide: how I talk upstream

## Who I am in threads

I am a first-time contributor in this course, working in the Path Review repo. I investigate one issue at a time, and I write what I actually ran and what I actually saw. Readers can expect a specific next step, then a report from my own machine, not a promise that the bug is already fixed.

## Rules I write by

### Rule: name this issue

The comment has to mention the behavior of this issue. A line that could be pasted onto any issue is not a claim.

- Wrong: "I'd like to be assigned to this. I can fix it."
- Right: "I'd like to investigate the orchestrator keeping a ContextManager across reviews, so a second review can return the first review's tool result."

### Rule: promise the next step, not a fix

Say what I will do next. Do not promise a merged fix, a root cause, or a date.

- Wrong: "I will have a fix up within two days, guaranteed."
- Right: "Next I will reproduce two reviews on one orchestrator and report whether the second run returns the first run's result."

### Rule: show the result

A reproduction comment includes the command output or the observation. Saying "reproduced" with nothing shown is not a report.

- Wrong: "I reproduced this, it happens every time."
- Right: "The second review returned the first review's cached tool result. The output is below."

### Rule: my own run

On a shared issue I still write my own steps and my own result. I do not attach myself to someone else's report.

- Wrong: "Same as above, can confirm."
- Right: "On my machine (macOS, Python version below) I ran the two reviews and got this output."

### Rule: disclose when the repo requires it

If the repo's policy says AI use must be disclosed, the comment says I used an AI assistant and that I ran the steps myself. If the policy does not require that, I do not add a disclosure paragraph for its own sake.

- Wrong: posting a report written with an assistant on a repo whose policy says all AI usage must be disclosed, and never saying so.
- Right: "I used an AI assistant to help me organize this report. I ran and checked every step myself."

## Things I never post

- A date by which a fix will be done.
- "Guaranteed", "this definitely works", or a root cause I have not shown.
- "Same as above, can confirm."
- A bare "+1" or "assign me" with no mention of the bug.
- A claim that I already reproduced the bug in the comment that only promises to try.
