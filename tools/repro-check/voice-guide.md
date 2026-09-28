# Voice guide: writing to maintainers

Personal rules for how I write claim comments and repro reports, with
wrong/right pairs. The skill checks drafts against these before I post.

## Rule 1: promise, don't assert, in a claim comment

A claim goes up before reproduction is done. It should say what I will
do, not what I have already proven.

- **Wrong:** "I've confirmed this bug and have a fix ready."
- **Right:** "I'd like to work on this — I'll reproduce it using the
  steps above and post a report with environment and observed output
  before starting on a fix."

## Rule 2: name the specific behavior, not the issue title

A generic claim gives the maintainer nothing to react to.

- **Wrong:** "I'll take this one!"
- **Right:** "I'll take on the vendored/build-output detection issue —
  reproducing with the file list above and confirming the primary
  language misclassification."

## Rule 3: state the outcome exactly as the evidence shows it, no more

Confidence language ("confirms", "exactly", "definitively") should never
exceed what the attached output actually demonstrates.

- **Wrong:** "This confirms the reported bug is present and affects the
  current release."
- **Right:** "Running the exact steps from the issue on the current
  release reproduces the same panic, with an identical stack trace."

## Rule 4: a classmate's or someone else's claim doesn't change my own claim

Post my own claim in my own words regardless of who commented first;
never defer to or reference another contributor's claim as a reason to
hold back.

- **Wrong:** "I saw someone else already commented on this, so I'll
  just add my notes here too."
- **Right:** "I'd like to work on this — I'll reproduce the issue
  described above and post a report before starting on a fix."

## Rule 5: an honest "could not reproduce" is written with the same
confidence as a successful reproduction — never hedged into vagueness

- **Wrong:** "Hmm, not totally sure, maybe this doesn't happen anymore?"
- **Right:** "I was unable to reproduce this on the current release
  (version X, environment below): running the exact steps from the
  issue produces [Y], not the panic described. I'm noting this here in
  case it's already been fixed, rather than assuming so."