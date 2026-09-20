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
| Active Repo | Repo facts block, the "last push to any branch" date | last push is within 90 days of the snapshot date | required |
| Issue Open Date | Issue section, the "opened by <user> on <date>" line | opened less than 90 days before the snapshot date or labels include "stale::recovered" | required |
| Assigned | Repo facts block, "this issue: assignees:" | assignees is none | required |
| Comments Checked | Comments subsection under the Issue section | no maintainer/owner comment says the issue is being closed | preferred |
| Has Fix | Whole bundle, presence of a "Fix" subsection | no Fix subsection is present | required |
| Good First | Issue section, the labels: line | labels include "good first issue" | preferred |

## Verdict rule

Accept if every required check passes; preferred checks do not effect verdict. If any required checks are unclear reject.

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->
