# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/174

**Branch**

fix/57-root-level-skip-patterns

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1 — 17/20 scored items agree (bar: 18/20: below the bar). Categories: clear-accept 1/7, silent-drift 4/4, standards-wall 2/2, not-tested 4/4, unreviewable 3/3. Disagreements concentrated in clear-accept: pkg-02 was the only clear-accept match, with the rest failing on "Test evidence is concrete, not asserted," "Description and checklist claims are proven," or "Description is reviewable against the diff" being too strict — these checks initially demanded proof at a level of literal detail (exact transcripts, exact line counts, claims fully re-derivable from a trimmed diff) that real, legitimate PRs don't provide.

Run 2 — 16/20 scored items agree (bar: 18/20: below the bar). Categories: clear-accept 3/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. First loosening pass (distinguishing routine checklist mechanics from substantive claims, and accepting automated-test-plus-suite-count as proof) fixed pkg-05 and pkg-08's immediate failures but surfaced two new ones (pkg-16, pkg-19) on "Description and checklist claims are proven," caused by demanding that a claim's exact output string be fully re-derivable from a diff's trimmed hunks.

Run 3 — 18/20 scored items agree (bar: 18/20: PASS). Categories: clear-accept 5/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. Fixed by distinguishing "the diff contradicts a claim" from "the diff merely doesn't fully prove a claim," and by adding a traced-input rule: a control-scenario claim is proven by the diff's own logic when the control's input doesn't satisfy a newly-added guard, so the pre-existing, unmodified code path it falls through to is proof enough without a separate transcript. pkg-16 and pkg-19 remain as known, understood misses — pkg-16 is a genuinely arguable case (a shared-function change with an unverified blast radius on a caller the diff doesn't show); pkg-19's miss reflects grading-model variance on a borderline package, confirmed by it passing cleanly in an earlier canary run with identical rubric text. This is the run recorded in the committed eval-run.txt.

**Package analysis**

pkg-16 (kubernetes/minikube#23471, category: clear-accept): gold = accept, my rubric's verdict = reject, failed checks = "Description and checklist claims are proven" and "Description is reviewable against the diff."

The PR's diff changes a shared function, `DoLoadImages`, to return a non-nil error whenever `len(succeeded)==0`. The description claims "the start-time image loading path keeps its tolerant behavior," but the diff shown doesn't include the start path's caller code, so there's no visible guard distinguishing the `image load` caller from the `start` caller. My rubric read this as the description omitting a change the diff makes elsewhere (silent drift into a shared function), since nothing in the visible diff proves the start path is unaffected. The gold label disagrees, likely because the thread (not fully visible evidence to my check) contains a maintainer exchange about exactly this risk, which a human grader may read as sufficient context to trust the author's claim even without seeing the start path's caller code. I consider this a genuinely arguable case rather than a clear rubric bug: tightening the check to accept it risks letting a real case of silent drift into a shared function through elsewhere, which is exactly what the "silent-drift" category (currently 4/4) is built to catch.

**Check rationale**

"A control-scenario claim is proven by the diff's own logic, without a separate transcript or test, when the control's stated input would not execute the diff's changed lines — trace the specific input through the diff: if the diff adds a new guarded branch (an early return or conditional) and the control's input does not satisfy that guard's condition, execution falls through to the pre-existing, unmodified code, and the control is proven by that fact alone; this reasoning applies per input, not per file or per function."

I added this after two accept-gold packages (pkg-08, pkg-19) failed on "Test evidence is concrete" for stating a control scenario ("a tracked change still prompts and stashes normally") with no separate transcript or test. My first attempt at a fix said this was fine when "the diff does not touch" the relevant code at all, but the grading model correctly read that literally — if the diff touches the same function at all (even in an unrelated branch), the leniency didn't apply, and both packages kept failing. I rewrote the rule to trace the specific claimed input through the diff's actual branching logic instead of reasoning at the file/function level, since a diff can add a new guarded branch while leaving a different branch's lines completely unexecuted for a given input — and that distinction, not "which file was touched," is what actually determines whether separate proof is needed.

**Trade-offs**

Loosening "Test evidence is concrete, not asserted" and "Description and checklist claims are proven" to accept automated-test-backed proof, routine checklist items proven by the diff's own visible content, and traced-input control-scenario claims, trades some strictness for correctly accepting real, well-evidenced PRs. Before treating each loosening as final, I canary-tested it against the reject-category packages (pkg-01, pkg-03, pkg-04, pkg-12 — one from each of standards-wall, silent-drift, not-tested, unreviewable) using `--only`, and all four continued to correctly reject across every revision; none of these categories regressed from 4/4, 4/4, 2/2, or 3/3. The known remaining miss, pkg-16, is a case I accept as a limitation: a rubric cautious enough to always require proof for a shared-function's blast radius will occasionally reject a PR a human reviewer, with fuller thread context, would accept.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
