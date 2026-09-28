# Evidence guide: where reproduction-package signals live

Every rubric check needs an evidence source. This guide maps the proof
families this skill judges to concrete locations in a reproduction
package: on github.com when checking your own draft by hand, and in the
snapshot bundle (`claim_comment`, `repro_report`, `issue`,
`thread_highlights`, `repo_facts` fields) in eval mode.

## Family 1: is the environment recorded, and are the steps followable?

A reproduction a stranger can't run is not proof, however confident the
prose.

| Signal                | On github.com (your own draft)                                                                                                                | In the eval bundle                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ |
| Environment specifics | the environment block at the top of your repro report: OS, language/runtime version, dependency versions, commit hash or branch               | `repro_report`'s environment section |
| Step completeness     | read your own numbered steps as if you had never seen the codebase: does each step name one literal action (a command, a file edit, a click)? | `repro_report`'s numbered steps      |
| No skipped jumps      | check for any step that assumes something not stated in an earlier step                                                                       | same field, read in order            |

Vague environment values ("latest", "my machine", "recent version") fail
this check even if the steps are otherwise numbered and complete —
specificity and completeness are both required, not either/or.

## Family 2: does the shown behavior match the issue?

A report can be perfectly followable and still prove nothing, if what it
reproduces is a different bug.

| Signal          | On github.com                                                                                                                      | In the eval bundle                                                                                            |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Symptom match   | your repro report's observed output/error, read side by side with the issue body's stated symptom                                  | `repro_report`'s output excerpt vs. `issue` body                                                              |
| Code path match | does the failure happen in the same function/feature the issue names, or an adjacent one that merely looks similar?                | same fields, plus `thread_highlights` for any maintainer clarification of the actual bug                      |
| Input fidelity  | did the reproduction use the same input/command the issue specifies, or a modified version that could trigger a different failure? | `repro_report`'s command/input, compared character-by-character against the `issue` body's reproduction steps |

A plausible-looking near-miss (same error class, different trigger; same
feature, different code path) still fails this check.

## Family 3: does the evidence back the claimed outcome?

"I reproduced it" and "I could not reproduce it" are both acceptable
outcomes — an evidenced negative is a pass. An unevidenced claim, either
direction, is not.

| Signal                  | On github.com                                                                                                                   | In the eval bundle                                                 |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Attached proof          | the actual output, log line, stack trace, or screenshot description backing the stated result                                   | `repro_report`'s result line, read against its own attached output |
| Confidence vs. evidence | does the report claim more certainty ("confirms", "exactly the class of failure") than the attached evidence actually supports? | same field                                                         |

## Family 4: is the claim specific, not generic?

A claim comment that could be pasted onto any issue tells the maintainer
nothing.

| Signal                      | On github.com                                                                                                                  | In the eval bundle    |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | --------------------- |
| Names the specific behavior | does the claim restate, in the claimant's own words, what will be investigated — not just the issue title or "I'll take this"? | `claim_comment` field |
| Promise, not assertion      | claims go up before reproduction is done: does it promise a report rather than assert a fix or a finished repro?               | same field            |

## Family 5: does the package follow the repo's stated conventions?

| Signal                    | On github.com                                                                                                     | In the eval bundle                            |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| AI-disclosure requirement | `CONTRIBUTING.md`, `.github/` docs, or a PR/issue template checkbox asking contributors to disclose AI assistance | `repo_facts`' contribution/AI-use policy line |
| Disclosure present        | does `claim_comment` and/or `repro_report` actually include that disclosure, if required?                         | `claim_comment` + `repro_report` text         |

Silence in `repo_facts` (no policy stated) passes by default.

## Reading the bundle honestly

The bundle's repo-facts and package text are captured on the date
stamped at the top of the file. Grade only what's in the bundle — do not
open the live issue; it has moved on since capture, and the gold label
describes the snapshot, not today's GitHub.
