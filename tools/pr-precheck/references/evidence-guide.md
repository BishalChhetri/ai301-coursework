# Evidence guide: where evidence lives in a PR package

## Diff matches the plan's scope

Where it lives: the Plan context section's "Files:" list (or, live, `plan.md`'s
Files to touch and `## Deviations`), read against the Diff's actual changed
files.

What good looks like: every file in the diff is named by the plan, or the
mismatch is explained in Deviations. "A changelog entry" in the plan counts
as naming the changelog file specifically. A diff that touches a file the
plan never mentioned, with no Deviations note, is silent drift — the PR is
doing something the plan didn't say, and nobody reviewing just the
description would know.

## Description and checklist claims are proven

Where it lives: every sentence in the Description and every checked box in
the Checklist, read against the Diff and the Test evidence.

What good looks like: a substantive claim — what the fix does, its scope,
what it deliberately leaves out — is not contradicted by a line in the diff
or by real test evidence. A claim of absence ("no options added") is proven
when nothing in the diff adds an option. Routine checklist mechanics that
don't describe behavior (formatting, internationalizing a string,
regenerating a generated file, docs marked N/A) don't need a separately
pasted command output — they're proven by what the diff itself visibly does
(a new string routed through the i18n table rather than hardcoded in place)
or by the absence of any contrary signal. Reserve the strict "show me the
command and its output" standard for claims about what the code actually
does, not for boilerplate housekeeping items.

## Test evidence is concrete, not asserted

Where it lives: the Test evidence section (live: `test_evidence.md`), and the
Diff's own added tests.

What good looks like: a test-plan item asserting a behavior change shows a
real before/after transcript — actual terminal output, not a sentence
describing what it would print. A control-scenario claim is proven by the
diff's own logic, with no separate transcript or test, when tracing the
control's stated input through the diff shows it never reaches the diff's
changed lines — most commonly, the diff adds a new guard (an early return or
an `if`) before pre-existing code, and the control's input doesn't satisfy
that guard, so it falls through to the exact unmodified lines that ran
before the fix. Judge this per input, not per file or function: a diff can
touch a function and still leave a given scenario's execution path
completely unaffected. This leniency never covers the scenario the diff's
changed lines actually execute for — that one always needs real proof. A
test-plan item, including a control scenario the changed lines do execute
for, is also proven by a specific automated test in the diff whose assertion
directly checks the mechanism being fixed — asserting an escaped or
generated string, a return value, or an error type counts as real proof even
through a mocked dependency. Pair that with a stated command and outcome for
the suite containing it; a bare "passes" is enough when the specific test is
named or its assertion is quoted.

## Description is reviewable against the diff

Where it lives: the Description, read line-by-line against the Diff.

What good looks like: a reviewer with no other context could match each
sentence in the description to something visible in the diff. A description
that's accurate but vague ("improved the printer logic") fails this just as
much as one that's wrong, because neither lets a reviewer verify the claim
without extra back-and-forth.

## Meets the repo's stated standards

Where it lives: `repo-facts`' own "pull requests" line names exactly what
that repo's template asks for — this varies by repo. Read it first, every
time, rather than assuming any fixed shape. Live mode: the repo's actual
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.

What good looks like: the candidate PR satisfies what this repo actually
asks for. A lightweight repo (a type-of-changes checklist, a changelog entry)
is fully satisfied by exactly that; a heavier template (Summary, Issue,
Changes, Testing, Screenshots, Notes for Reviewers, as Path Review's own
template asks in live mode) needs all of its sections filled. Never grade a
repo against a template it doesn't use. A routine mechanical requirement
(formatting, codegen regeneration, i18n) is satisfied by what the diff
visibly shows, the same as in "Description and checklist claims are proven"
— it doesn't need a separately pasted command output. Any checked checklist
box needs support elsewhere in the package; anything CONTRIBUTING requires
(a changelog entry, an AI-use disclosure) must be both stated and actually
present in the diff. Silence on a given point passes that point by default.

## A diff is a trimmed excerpt, not a complete program

Every diff in a PR package is a unified-diff hunk with limited context lines
— it never contains a full function body, full call signatures, or the
implementation of helper functions it calls. When checking a claim or a
transcript against the diff, distinguish two different things: whether the
diff contradicts the claim (a different code path, a condition that can't
produce the stated result, a change the diff doesn't make at all), versus
whether the diff merely doesn't fully prove every character of the claim.
Only the first is a real failure. A transcript's exact printed string often
depends on code outside the visible hunk (a variable defined above the shown
context, a helper function's formatting) — that's expected and not a sign of
a fabricated result.
