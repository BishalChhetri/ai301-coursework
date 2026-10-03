# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: the package's **Issue** section (title and reported behavior), the
**Thread highlights** (any competing or unverified claims about cause), and the
**Repro evidence** section (the actual observed steps/timings/output). The plan's
own **Diagnosis** subsection states the claimed cause and should cite specific
lines from Repro evidence, not just a thread comment.

What good looks like: the stated cause explains every observation in Repro
evidence, including any step that rules out an alternative (e.g. a timing run with
a component removed from the loop entirely). A diagnosis that rests only on an
unverified thread comment, while an available repro step contradicts that theory,
fails this — citing a thread claim is not the same as citing evidence.

## Scope

Where it lives: the candidate plan's **Scope** subsection — its "in scope" and
"not in scope" statements.

What good looks like: every area to be touched is named with a reason tied to the
diagnosis; at least one boundary is explicit. When the diagnosis itself is wrong,
scope built on it often excludes exactly the area the evidence points to (e.g.
explicitly ruling out "the highlighting pipeline" when the evidence shows
highlighting is the actual bottleneck) — a mismatch between Scope's stated
boundary and what Repro evidence actually points to is a signal worth tracing back
to Diagnosis.

## Executability

Where it lives: the candidate plan's **Changes** subsection — the numbered steps
describing what will actually be done.

What good looks like: a stranger could start implementing from these steps alone
— each names a specific function, file, or mechanism, not a vague area. Change
size is proportionate to the bug as described.

## Test plan

Where it lives: the candidate plan's **Test plan** subsection, read against
**Repro evidence**'s exact steps and timings.

What good looks like: the test plan names a concrete, observable before/after
(e.g. "lands on the last line immediately, matching less") rather than a vague
"verify it works." A test plan can be concrete and still fail to verify the real
bug if it's built on a wrong diagnosis — note this, but grade "Test plan is
concrete" on its own terms; let the Diagnosis check catch the underlying mismatch.

## Honesty

Where it lives: a stated Risks/Unknowns area in the candidate plan (if present),
and, once built, a `## Deviations` section in `plan.md`.

What good looks like: the plan's confidence matches what the evidence actually
supports. A package with Thread highlights showing a live, unresolved competing
explanation (e.g. the reporter's own observation pointing a different direction)
but a plan stated with full certainty and no risks/unknowns section at all is a
clear failure here — confidence outpacing the evidence.

## Comms

Where it lives: the **Candidate plan comment**, read against **Thread highlights**
and the **Repo facts** block's contribution policy.

What good looks like: the comment engages with the thread's live signals — not
just the one comment that happens to support the chosen diagnosis, but any
competing or clarifying signal from the issue's own reporter or a maintainer. A
comment that cites only the thread comment matching its chosen (possibly wrong)
diagnosis, while silently skipping the reporter's own competing observation, reads
as cherry-picked rather than thread-aware. Also check the Repo facts contribution
policy for any AI-use disclosure requirement, same as Unit 2; silence passes by
default.
