# Procedure: how to grade a PR package

## Read order

1. Read the issue's title and body first.
2. Read the Plan context (the plan the PR claims to implement), including any
   `## Deviations` note.
3. Read the repo-facts block's stated PR requirements fresh, every time — do
   not assume any fixed template shape; different repos ask for different
   things, from a full multi-section template to just a checklist and a
   changelog entry.
4. Read the Diff in full.
5. Read the Description and Checklist.
6. Read the Test evidence last, so the claims it's meant to support are
   already fresh.

## Evidence gathering

1. From the plan, extract the stated Files to touch and any Deviations note.
2. From the Diff, extract the actual list of changed files and, for each,
   what changed.
3. Compare the plan's file list against the diff's file list: note any file
   in one but not the other, and whether Deviations explains the mismatch.
4. From the Description and Checklist, extract every discrete claim —
   including a checked box and a claim of absence ("no options added").
5. For each claim, search the Diff and Test evidence for a specific backing
   line (a diff hunk, a command plus its printed output, or a specific
   automated test added in the diff whose assertion matches the claim,
   combined with a real suite-run result). A claim with no such line found is
   unproven.
6. From the repo's stated PR requirements (step 3 above — the bundle's
   repo-facts in eval mode, or the live repo's actual PR template and
   CONTRIBUTING in live mode), extract every requirement that repo actually
   states. Grade only against what this repo asks for, never a template it
   doesn't use.
7. In live mode only, also read the branch name and the description's issue
   reference, and check the branch has a type prefix, the issue number, and a
   short slug, and that the description names the same issue number. This
   step does not apply in eval mode, since no eval bundle carries a branch
   name.

## Check execution

1. Grade each check P, F, or ? using the gathered evidence and that check's
   pass condition.
2. If a claim's proof is completely absent, grade F, not ? — absence of
   evidence for a stated claim is a failing condition, not an ambiguous one.
3. Grade ? only when evidence exists but is genuinely ambiguous (e.g. a
   command is shown but its output is cut off or illegible).

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check
   passes.
2. Treat any ? as a fail.
3. State the verdict as accept or reject, and name which required check(s)
   failed, if any, quoting the specific unproven claim or mismatched file.
