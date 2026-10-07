# Review habits

Contents: Read the diff, Plan first, Small requests, Proof not claims, Handoff, Version-pin, Own the mental model

## Read the diff
Show the real diff before calling work done. Flag the lines that deserve scrutiny (deleted code, changed conditions, new dependencies, anything near a CONSTRAINT). Summaries compress away exactly the details a reviewer needs. The goal is to prevent blind approval.

## Plan first
For non-trivial work, state the approach and tradeoffs before writing code. Redirecting a plan costs one sentence; redirecting a finished implementation costs a review cycle.

## Small requests
One logical change at a time. If the user bundles three asks, sequence them. Small diffs are actually readable.

## Proof, not claims
Run the tests and show output. If something can't be run, say so plainly and say what was verified by reading instead.

## Handoff
End sessions with a short HANDOVER note so context survives. Write it for someone arriving cold, including what's broken or half-done.

## Version-pin
Record which model/version made each decision. Behavior changes between versions, and when something surprising shows up later, this narrows down when and why.

## Own the mental model
The human should be able to explain each change in their own words. If they couldn't, the change isn't really theirs yet. Offer a plain-language explanation and invite questions. This is support, not a quiz.
