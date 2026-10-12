---
name: pr-precheck
description: Grade a PR package (a candidate pull request's title, description, diff, and test evidence, read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking a draft PR before opening it, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: is this
ready to submit? A package is a candidate pull request — its title,
description, diff, and test evidence — read against the plan it claims
to implement and the issue that plan belongs to. You do not answer
from gut feel, and you do not answer from a different question than
this one: you answer by executing the student-authored grading
procedure in `procedure.md`, which applies the rubric in `rubric.md`
to evidence gathered per `references/evidence-guide.md`.

## Inputs

One of:

- **Live mode**: the student's own `plan.md` (deviation notes
  included), the diff on their branch (`git diff main...HEAD`), their
  draft PR title and description (`pr_draft.md`), and their test
  evidence (`test_evidence.md`), read against their issue. Gather
  issue-side evidence live — the issue thread, the repo's PR template,
  and its stated contribution policy — the same way `references/evidence-guide.md`
  says to. A student on the house chain has no plan or repro pack of
  their own to point back to: there, the plan and repro pack the staff
  supplied stand in for the student's own, and the same checks grade
  the same things. Read the drafts the way a maintainer reviewing the
  actual pull request will: the package is what the diff, the
  description, and the evidence file contain, not other files sitting
  in the student's working directory.
- **Eval mode**: a package bundle (a markdown file containing the
  issue context, a repo-facts block, the plan the PR claims to
  implement, the PR's diff, its title and description, and its test
  evidence). Use ONLY the bundle text as evidence. Do not fetch
  anything; the bundle is the whole world. Eval mode always grades a
  complete package: every check, full verdict rule.

## The scope sets the field (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. It names where the student's PR must live and the house rules
that apply there; refuse to grade a PR outside the scoped repo. If the
scope's repo line still carries an unfilled placeholder, stop without
grading and tell the student to fill the `Repo:` line in `scope.md`
with their section's Path Review repo; never guess a scope. In eval
mode, ignore `scope.md` entirely.

## The voice guide gates outgoing words (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream, carried forward from week 2, with wrong/right
examples. Hold the outgoing PR title and description against those
rules, reporting any rule the draft breaks in the summary, quoting the
rule. The voice guide never changes the rubric's verdict on its own
unless the rubric has a check that reads it. In eval mode, ignore
`voice-guide.md` entirely: voice is personal and carries no gold
labels; universal communication-quality checks live in the rubric.

## The rubric is the brain

Read `rubric.md`. It defines:

1. A table of checks. Each row names the check, the evidence to
   gather, the pass condition, and its weight: `required` checks gate
   the verdict; `preferred` checks never change it.
2. A verdict rule: how check results combine into a final verdict,
   including what `unclear` does.

`references/evidence-guide.md` is the rubric's map: where each kind of
evidence lives in a PR package — in the plan, the diff, the test
evidence, the description — and, in live mode, on GitHub, and what
good looks like there.

## The procedure is the hands

This file does not tell you the workflow: **execute `procedure.md`**.
That file is the skill's operating procedure, written by the student,
and it must decide the read order, how each evidence family gets
gathered, how a check executes against gathered evidence (most of this
week's checks compare two things side by side — the diff against the
plan, the test evidence against the plan's test plan, the description
against the diff), and how check grades become the verdict. Follow it
as written, the same way an executor follows a rubric: exactly,
without improvising around gaps. Where the procedure is silent, note
the gap in your summary rather than silently inventing a step; a
procedure gap is feedback the student needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so: this skill cannot grade without a rubric
AND a procedure, and that is by design. The instruction comments
inside the shipped templates are guides, not content — ignore comments
when checking whether a file is still a template; only written text
counts. The rubric, the evidence guide, and the procedure are the
parts the student writes this week; the voice guide carries over from
week 2.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject`
(hold). There is no third verdict, no "accept with reservations", no
score. Reservations belong in check evidence lines, not in the
verdict. Emit a fenced JSON block, then nothing else after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {
      "name": "<check name>",
      "grade": "pass|fail|unclear",
      "evidence": "<one line: the fact or quote that decided it>"
    }
  ],
  "verdict": "accept|reject"
}
```

Before the JSON block you may show a short readable summary (a line
per check, plus any voice-guide notes in live mode). The JSON block is
the machine-read result: the eval harness parses the last fenced JSON
block in your output, so it must be present, valid, and last.

## Grading discipline

- **Evidence first.** No check is graded without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be
  ready and a beautiful confident one can be hiding drift. Every check
  reads the artifact itself — the diff, the test evidence, the
  description — against the plan, the issue, and the stated standards,
  never the formatting.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not the run.** Follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs; where the rule is silent, an unverifiable
  claim is a failing one. A PR you cannot verify from the package is a
  PR that is not ready to submit.
