# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check                                       | Evidence                                                                                                        | Pass condition                                                                                                                                                                                                                                                                                                                                                                           | Weight   |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Diagnosis grounded in evidence              | `plan.md`'s Diagnosis section, read against the issue body and the quoted repro evidence                        | Passes if the plan states a specific root cause, quotes the repro evidence it's based on, and that cause is consistent with what the issue and repro evidence actually show; fails if no cause is stated, no evidence is quoted, or the stated cause contradicts or ignores the evidence (e.g. picking one cause when the evidence doesn't distinguish between competing causes)         | required |
| Scope is bounded and justified              | `plan.md`'s Scope section                                                                                       | Passes if the plan names each file or area it will change, explicitly states what it will not touch, and gives reasoning tying the changes to the diagnosis; fails if any of the three is missing                                                                                                                                                                                        | required |
| Executable without the author               | `plan.md`'s files-to-touch / approach section                                                                   | Passes if a stranger could start work without asking the author anything — each file to modify is named with a reason tied to the diagnosis, any add/delete is justified, and the change reads as minimal (roughly under 500 lines) for the bug described; fails if a file has no stated reason, an add/delete is unjustified, or the approach is vague about what will actually be done | required |
| Test plan is concrete                       | `plan.md`'s Test plan section                                                                                   | Passes if the plan states how success will be observed — re-running the repro steps with a stated expected post-fix result, or an added automated test — and names the specific commands/assertions involved; fails if the test plan is vague ("will test it") or absent                                                                                                                 | required |
| Honest about risks and unknowns             | `plan.md`'s Risks/unknowns section, and its `## Deviations` section if the plan has been built                  | Passes if stated confidence matches what's actually known — risks and unknowns are named rather than glossed over, and any deviation from the original plan is recorded in the author's own words; fails if the plan claims certainty the evidence doesn't support, or a deviation clearly occurred but isn't recorded                                                                   | required |
| Comms match the thread and repo conventions | `comment.md`, read against the issue's thread/maintainer signals and the repo-facts block's contribution policy | Passes if the plan comment addresses any open question or maintainer signal already in the thread (not boilerplate that ignores it) and follows the repo's stated contribution policy, including any AI-use disclosure requirement; fails if the comment ignores a live thread signal it should address, or omits disclosure the policy requires                                         | required |

## Verdict rule

Accept (ready) only if all required checks pass. A grade of `unclear` (`?`) on any check counts as fail. No check is `preferred` in this rubric.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
