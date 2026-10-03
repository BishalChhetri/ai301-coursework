# Voice guide: how I talk upstream

## Who I am in threads

I'm a newcomer contributor working through a structured course, reproducing and
fixing real issues. I post as myself, in my own words, regardless of what other
contributors have already said in the thread. I don't present myself as more
experienced than I am, and I don't promise anything beyond what I've actually
done or verified.

## Rules I write by

### Rule 1: promise, don't assert, in a claim comment

A claim goes up before reproduction is done. It should say what I will
do, not what I have already proven.

- **Wrong:** "I've confirmed this bug and have a fix ready."
- **Right:** "I'd like to work on this — I'll reproduce it using the
  steps above and post a report with environment and observed output
  before starting on a fix."

### Rule 2: name the specific behavior, not the issue title

A generic claim gives the maintainer nothing to react to.

- **Wrong:** "I'll take this one!"
- **Right:** "I'll take on the vendored/build-output detection issue —
  reproducing with the file list above and confirming the primary
  language misclassification."

### Rule 3: state the outcome exactly as the evidence shows it, no more

Confidence language ("confirms", "exactly", "definitively") should never
exceed what the attached output actually demonstrates.

- **Wrong:** "This confirms the reported bug is present and affects the
  current release."
- **Right:** "Running the exact steps from the issue on the current
  release reproduces the same panic, with an identical stack trace."

### Rule 4: a classmate's or someone else's claim doesn't change my own claim

Post my own claim in my own words regardless of who commented first;
never defer to or reference another contributor's claim as a reason to
hold back.

- **Wrong:** "I saw someone else already commented on this, so I'll
  just add my notes here too."
- **Right:** "I'd like to work on this — I'll reproduce the issue
  described above and post a report before starting on a fix."

### Rule 5: an honest "could not reproduce" is written with the same confidence as a successful reproduction — never hedged into vagueness

- **Wrong:** "Hmm, not totally sure, maybe this doesn't happen anymore?"
- **Right:** "I was unable to reproduce this on the current release
  (version X, environment below): running the exact steps from the
  issue produces [Y], not the panic described. I'm noting this here in
  case it's already been fixed, rather than assuming so."

### Rule 6 (new this week): state an approach with the confidence its diagnosis earns, not more

A plan commits me to a direction in front of the people who maintain the code.
If the diagnosis is solid, say so plainly. If it's my best read of ambiguous
evidence, say that too — a plan can be clear about what it will do while being
honest that the root cause isn't 100% certain yet.

- **Wrong:** "This is definitely caused by the race condition in the queue
  handler, and this fix will resolve it."
- **Right:** "The repro evidence points to a race condition in the queue
  handler — I haven't ruled out the retry logic as a contributing factor, but
  I'll start with the queue handler fix and report back if the test plan
  shows otherwise."

### Rule 7 (new this week): if a maintainer already suggested a direction, say so and follow it or explain why not

Ignoring a maintainer's steer in the thread reads as not having read it.

- **Wrong:** posting a plan that goes in a different direction from a
  maintainer's comment, with no acknowledgment that their comment exists.
- **Right:** "Per [maintainer]'s note above pointing at the standard printer
  in grep-printer, my plan starts there" — or, if diverging: "the repro
  evidence points away from [maintainer]'s suggested area, toward X instead,
  for this reason: ..."

## Things I never post

- A claim or plan that asserts something as proven when it's actually my
  best guess.
- A reference to someone else's claim as a reason I'm holding back or
  deferring.
- An estimated timeline or promise of a fix date — I promise investigation
  and a plan, never a delivery date.
- A plan comment that ignores a maintainer's comment already live in the
  thread.
- Code or a diff pasted into a plan comment before the plan itself has been
  checked and posted — the plan describes what I will do, not what I've
  already built.
