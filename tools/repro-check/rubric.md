# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check                              | Evidence                                                                                                       | Pass condition                                                                                                                                                                                                                                                                                                                                                                                                                                            | Weight   |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Environment & steps followable     | `repro_report`'s environment record (OS, runtime/dependency versions, commit or branch) and its numbered steps | A stranger with the stated environment could execute every step as written with no missing action, no guessed setup, and no unstated jump from action to result; fails if any environment detail is vague ("recent version", "my machine") or a step is skipped                                                                                                                                                                                           | required |
| Behavior matches issue             | `repro_report`'s shown output/error, read against the `issue` body and `thread_highlights`                     | The specific output, error type, or code path shown is the same one the issue describes — not a different failure that merely looks similar (e.g. a different error triggered by a typo'd or altered input); fails on any mismatch, even a plausible-looking one                                                                                                                                                                                          | required |
| Evidence backs the claimed outcome | `repro_report`'s stated result line, read against its own attached output                                      | Passes an evidenced "reproduced" and an evidenced "could not reproduce" alike; fails when the stated outcome has no concrete output/log/error attached, or when the report's conclusion claims more than its own shown evidence supports                                                                                                                                                                                                                  | required |
| Claim is specific                  | `claim_comment` field                                                                                          | Names the issue's specific behavior in the claimant's own words — not a copy of the title, not a generic "I'll take this" with no detail; fails only on generic or templated text with no reference to the issue's actual behavior                                                                                                                                                                                                                        | required |
| Disclosure compliance              | `repo_facts`' contribution/AI-use policy, read against `claim_comment` and `repro_report` text combined        | Fails only when `repo_facts` states an explicit requirement that contributors disclose or note their AI use, and neither `claim_comment` nor `repro_report` contains that disclosure anywhere in the package; passes when the stated policy only sets conditions on AI use (human-authored comments, understanding, testing, responsibility) without requiring an explicit disclosure statement, and passes by default when no AI policy is stated at all | required |

## Verdict rule

Accept (ready) only if all required checks pass. A grade of `unclear` (`?`) on any check counts as fail. No check is `preferred` — an evidenced-but-generic claim, or a well-formatted report with no real proof, still isn't ready to post.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
