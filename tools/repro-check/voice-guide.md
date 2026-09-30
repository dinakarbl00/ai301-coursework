# Voice guide: how I talk upstream

## Who I am in threads

I am a student contributor who is still getting comfortable working in unfamiliar open-source codebases. I try to be clear about what I have actually tested and avoid sounding more certain than the evidence supports. When I comment on an issue, readers should be able to tell what I plan to do, what I tried, and what happened.

## Rules I write by

### Rule: Say what I will investigate, not what I guarantee

When claiming an issue, I describe what I plan to investigate without promising that I will fix it or finish by a certain date.

- Wrong: "I will fix this bug by tomorrow."
- Right: "I'd like to investigate this issue and reproduce the reported failure. I'll follow up with what I find."

### Rule: Separate evidence from conclusions

I only say that I reproduced a bug when the steps and output actually show the reported behavior. If the result is uncertain or different, I say that directly.

- Wrong: "I reproduced the issue successfully."
- Right: "I followed the reported steps and saw the same ZeroDivisionError when the index was empty."

### Rule: Include enough detail to rerun my result

When reporting a reproduction, I include the relevant environment, steps, command or input, and observed output instead of only saying that something worked or failed.

- Wrong: "I tested this and got the same error."
- Right: "Using Python 3.x, I created an empty index and ran the keyword search path. The call raised ZeroDivisionError before returning a result."

### Rule: Keep the comment specific to the issue

I refer to the actual behavior, file, or scenario from the issue instead of posting a generic claim or reproduction message.

- Wrong: "I'd like to work on this issue."
- Right: "I'd like to investigate the empty-index ZeroDivisionError in the BM25 keyword search path and follow up with a reproduction report."

### Rule: Do not pretend to know more than I do

If I am still investigating the root cause, I do not present a guess as a confirmed explanation.

- Wrong: "The bug is definitely caused by the BM25 library."
- Right: "The failure appears to happen when the search path receives an empty index; I'll verify the exact cause while reproducing it."

## Things I never post

- A promise that I will definitely fix the issue.
- A deadline I have not been asked to commit to.
- A claim that I reproduced something without showing evidence.
- "Same as above" or another person's reproduction copied as my own.
- A guessed root cause stated as fact.
- A long AI-sounding explanation when a short factual comment is enough.