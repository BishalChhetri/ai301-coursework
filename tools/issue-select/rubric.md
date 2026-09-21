# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | Repo facts: "last 5 default-branch commits" (author + date) and "maintainer first-response sample"; Comments section author_association values (OWNER/MEMBER/COLLABORATOR) | At least one of the last 5 default-branch commits was authored by a human (not a `[bot]` account merging a bot-originated change) within 60 days of the bundle's capture date, OR the maintainer first-response sample shows a reply from someone with OWNER/MEMBER/COLLABORATOR association within 30 days of capture | required |
| Unclaimed | Repo facts: "this issue: assignees:" and "linked PRs:" (with state per PR); Comments section for claim language | The assignees field is empty, no linked PR is in an "open" state, and no comment in the thread contains claim language ("I'll take this," "working on this," "can I work on this") posted within 14 days of the bundle's capture date without a maintainer response releasing the claim | required |
| Scope fits you | Issue body, comment thread, label list, and issue history (prior closed/unmerged PRs against it) | Fails if the issue is an umbrella/tracking issue, the thread shows an unresolved design debate with no maintainer decision, a maintainer states the fix touches core internals, the issue is a pure usage/support question, or the issue's history shows repeated abandoned attempts; otherwise passes regardless of how sparse the writeup is | required |
| Repo in use | Repo facts: "latest release," "last push to any branch," "archived:" flag, and stars count on the repo line | Fails immediately if archived: is true; otherwise passes if the latest release is within 12 months of the bundle's capture date, or last push to any branch is within 90 days of capture | required |
| AI contribution allowed | Repo facts: "contribution policy" line (summarizing `CONTRIBUTING.md`, `.github/` docs, dedicated AI policy files, and PR/issue template disclosure checkboxes) | Fails only if the policy states an outright ban on AI-generated contributions; disclosure, personal-understanding, testing, or human-review conditions still pass, and silence (no policy stated) passes | required |

## Verdict rule

Accept only if all five required checks pass. A fail on any single required check rejects the issue — there are no preferred checks in this rubric, since the assignment's category floor means none of the five families (maintainer, claim status, scope, repo activity, contribution policy) can be treated as optional. Any check graded `unclear` (`?`) counts as fail for that check, which means it rejects the issue on its own, since every check here is required.
