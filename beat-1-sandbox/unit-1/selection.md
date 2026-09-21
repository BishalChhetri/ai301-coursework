# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57

**Verdict output**

```
All three are accept — every required check passes on each. Ranking below is not a fit ranking: the fit profile in scope.md is still the unedited placeholder ("(Write a few sentences here.)"), so I ranked on rubric-visible signal only — tier label, and how sharply the issue defines "done." Fill in that section if you want a real fit ordering.

1. #57 — Tech detector counts vendored and build-output files (bug, good first issue, tier-1)
Sharpest definition of done of the three: it ships reproduction steps and names the two tests that must go green (test_node_modules_excluded, test_build_directory_excluded), so you know you're finished without asking anyone.

One thing to see clearly: ApoorvThite commented today "I'll take this one — working on it." Their author_association is NONE — a classmate, not a maintainer — so the Path Review house rule in scope.md applies and this does not block the issue. Comment your own claim and go. Outside this classroom repo, that same comment would have sunk the Unclaimed check.

2. #12 — Snapshot tests for prompt templates (enhancement, good first issue, tier-1)
Clean and untouched: zero comments, zero timeline events beyond labeling. Slightly looser than #57 — it names one file (tests/unit/test_prompt_templates.py) but leaves the version-bump mechanism for you to design.

3. #34 — X-RateLimit-Remaining header (enhancement, api, tier-2)
Passes every required check, and I want to flag the tension rather than hide it: it is tier-2, carries no good first issue label, and spans three areas including wiring new middleware into api/main.py. My rubric's Scope check fails an issue only on an umbrella, an unsettled design debate, a maintainer statement that the fix touches core internals, a support question, or abandoned attempts — none apply here, so it passes as written. If I want difficulty tier to actually count, that belongs in rubric.md as a condition, not in this run.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by human Aburke225 (COLLABORATOR), 5 days before today — within 60 days."},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees empty, no linked PRs in timeline; only claim is ApoorvThite (author_association NONE, a classmate) 2026-09-21 — scope.md Path Review house rule says student claims do not block."},
      {"name": "Scope fits you", "grade": "pass", "evidence": "Bounded bug with repro steps and two named failing tests (test_node_modules_excluded, test_build_directory_excluded); labels bug/good first issue/tier-1; no umbrella, debate, or prior attempts."},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16, 5 days before today — within 90 days (no releases exist)."},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md mentions no AI/LLM policy and the PR template has no disclosure checkbox — silence passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/12",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by human Aburke225 (COLLABORATOR), 5 days before today — within 60 days."},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees empty, 0 comments, timeline holds only 6 labeled events by Aburke225 — no linked PRs and no claim language."},
      {"name": "Scope fits you", "grade": "pass", "evidence": "Single bounded task naming tests/unit/test_prompt_templates.py at 3-5 hours; labels good first issue/tier-1; not an umbrella, debate, or support question."},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16, 5 days before today — within 90 days (no releases exist)."},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md mentions no AI/LLM policy and the PR template has no disclosure checkbox — silence passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/34",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by human Aburke225 (COLLABORATOR), 5 days before today — within 60 days."},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees empty, 0 comments, timeline holds only 3 labeled events by Aburke225 — no linked PRs and no claim language."},
      {"name": "Scope fits you", "grade": "pass", "evidence": "One bounded feature (middleware calling existing check_rate_limit) at 3-5 hours; no maintainer statement that it touches core internals, no umbrella, debate, or prior attempts — tier-2 alone is not a fail condition in the rubric."},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; last push to any branch 2026-09-16, 5 days before today — within 90 days (no releases exist)."},
      {"name": "AI contribution allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md mentions no AI/LLM policy and the PR template has no disclosure checkbox — silence passes."}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Run 1 — 18/20 scored items agree (bar: 18/20 → PASS). Category tallies: claimed 4/4, clear-accept 7/8, dead-repo 3/3, policy 1/1, scope 3/4. This was the only run performed; the committed `eval-run.txt` records this run.

**Issue analysis**

`issue-19` (`zxcalc/zxlive#517`, "Selecting large subgraphs in proof mode freezes the UI"): gold = accept, my rubric's verdict = reject, failed check = "Scope fits you."

The issue's body lists two possible causes ("matchers are slow for certain rewrites" and "UI update is waiting for the matching thread to finish") and then three "additional suggestions," including moving rewrite matching to multi-processing and applying rewrites in a separate thread. My Scope check reads this as an unresolved design debate with no maintainer decision: the issue's own author, RazinShaikh, is a COLLABORATOR, and rather than committing to one fix he laid out several architectural options without picking one. That trips two of my check's fail conditions at once — no maintainer decision among competing approaches, and a maintainer describing changes at the level of the app's core matching/threading architecture.

The gold label disagrees because the actual reproducible bug — the UI freezing when selecting large subgraphs — is itself narrow and well-defined, even though the issue also lists optional enhancement ideas alongside it. A newcomer doesn't have to implement all three suggestions to close the issue; picking one bounded piece (e.g. moving the matching work off the UI thread) would do it. My Scope check can't distinguish "a maintainer floated several optional improvements next to one narrow, reproducible bug" from "a maintainer genuinely can't decide how to fix this and it touches core internals" — it penalizes the issue for its breadth even though the required fix underneath it is small.

**Check rationale**

> "Unclaimed | Repo facts: "this issue: assignees:" and "linked PRs:" (with state per PR); Comments section for claim language | The assignees field is empty, no linked PR is in an "open" state, and no comment in the thread contains claim language ("I'll take this," "working on this," "can I work on this") posted within 14 days of the bundle's capture date without a maintainer response releasing the claim | required"

I wrote this check to catch the most common way a "first issue" quietly stops being available: someone else has already started, whether or not the repo's `assignees` field reflects it. Assignment fields are frequently stale or unused, so the check also scans comment text for common claim phrasing, and gives it a 14-day window so an old, abandoned claim doesn't block the issue forever. It is `required` because working on an issue someone else already owns wastes the assignment's clock, and I treated any unresolved claim signal as disqualifying rather than as a tie-breaker, since I'd rather lose a viable issue than walk into a collision.

**Trade-offs**

The check as written doesn't distinguish who is doing the claiming. When I ran it live on issue #57, ApoorvThite (a classmate, `author_association: NONE`) had commented "I'll take this one — working on it" that same day — exactly the claim language the check is built to catch. My rubric would have flagged it, and only the Path Review course's own house rule (documented separately in `scope.md`, outside my rubric) kept the issue usable. So the check gives up handling the in-classroom case correctly on its own: it can't tell a fellow student's placeholder claim inside this course repo from a real claim on an outside project, and it depends on an external rule to not reject a perfectly available issue. I chose not to special-case "classmate claims" inside the rubric itself, since that logic only makes sense for this specific course repo and would be dead weight (or actively wrong) on any other repo the skill is pointed at later.

---

## Selection rationale

**Selection rationale**

1. #57 is scoped narrowly enough (two named failing tests, clear repro steps) that it fits the time I have for this unit — it's a contained bug fix rather than an open-ended design task.
2. My rubric correctly confirmed the mechanical checks (no assignee, no maintainer objection, active repo, no AI-contribution ban). What it couldn't judge was that ApoorvThite's comment was a fellow classmate's placeholder claim rather than a real blocking claim on an outside project — that read was mine, based on the Path Review house rule, not something the rubric itself weighed.
3. Since a classmate already commented on the issue, I expect to post my own claim comment and briefly acknowledge theirs so there's no confusion about who's picking it up — a small social step rather than a technical obstacle.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
