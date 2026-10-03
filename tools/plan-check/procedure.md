# Procedure: how this skill grades a plan package

# Procedure: how to grade a fix plan

## Read order

1. Read the issue's title and body first.
2. Read the Unit 2 repro evidence quoted in or linked from the plan (the observed output/error, and the root cause if stated).
3. Read `plan.md` in order: Diagnosis, Scope, files to touch, Test plan, Risks/unknowns.
4. Read `comment.md` last.

## Evidence gathering

1. From the issue, extract the title and the specific behavior being asked about.
2. From the repro evidence, note the exact observed output. If multiple repro attempts show identical output, note that the repro evidence alone may not distinguish between competing causes — check whether the plan's diagnosis accounts for this or just asserts one cause without ruling out others.
3. From `plan.md`, extract: the stated diagnosis and the evidence it quotes; the files it will and won't touch, with reasoning; the test plan.
4. From `comment.md`, extract the diagnosis and scope as summarized to the maintainer.
5. Compare the plan's diagnosis and scope against the issue and the repro evidence for consistency.

## Check execution

1. Grade each check P, F, or ? using the gathered evidence and that check's pass condition.
2. If the needed evidence is entirely absent (e.g., no test plan at all), grade F, not ?.
3. Grade ? only when the plan addresses the topic but leaves genuine ambiguity a grader can't resolve from the text alone (e.g., "I'll add a test" with no detail on whether it's automated or manual).

## Verdict assembly

1. Apply the rubric's verdict rule: ready only if every required check passes.
2. Treat any ? as a fail.
3. State the verdict as ready or hold, and name which required check(s) failed, if any.
