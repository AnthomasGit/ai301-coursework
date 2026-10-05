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

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis | The plan's stated cause and its planned change, read against what the repro evidence shows | Passes if the stated cause sits in the component and trigger the repro evidence localizes it to, nothing in the evidence contradicts it, and the planned change fixes that cause rather than patching the place where the symptom shows up. A repro can't pin the code-level mechanism inside that component, so a mechanism consistent with the pinned facts counts as supported. | required |
| scope | The plan's scope statement (what it will and won't change), read against where the repro evidence places the cause | Passes if it names where the change goes (files, or a component narrow enough that a stranger knows where to look), says what it won't touch, keeps the change to the area the repro evidence implicates, and doesn't exclude the area where the evidence places the cause. Deferring a related variant or a secondary factor (one the evidence correlates with the symptom but doesn't place the cause in) is fine when the plan states the deferral and a reason, and the planned change still fixes the reproduced failing step. Leaving exact files or functions to be pinned later is fine if the plan says so and says how it will pin them. | required |
| approach | The plan's implementation steps, read against its scope statement | Passes if a stranger could start executing the steps without asking the author, and every step stays within scope. An investigation step (e.g. "trace with debug logging to pin the function") counts if it says what to do and what it will determine. | required |
| test | The plan's test plan, read against the repro evidence's steps | Passes if the plan names a verification that re-runs the repro evidence's failing step (an automated test, or the repro steps re-run with a stated expected outcome) that would observably fail on the current code and pass once the fix is in. Fails if the only verification is that the build passes, unrelated tests still pass, or something the repro never showed. | required |
| unknowns | The plan's load-bearing claims (the stated cause, and that the planned change removes the reproduced symptom), read against what the repro evidence and repo facts establish | Fails if the plan states as fact a load-bearing claim that the evidence contradicts, or a cause the evidence doesn't localize at all, without marking it as an assumption or open question. Otherwise passes. A code-level explanation that is consistent with the pinned facts and sits inside the component they localize is not an unknown. Neither are implementation details (line numbers, function names, counts of call sites, version history) or who in the thread suggested what. | required |
| conventions | The plan comment, read against the repo facts' contribution policy and the thread's maintainer or collaborator direction | Passes if the comment meets every mandatory requirement that applies to issue comments (e.g. an AI-use disclosure naming the tool and extent, a required format), and doesn't propose an approach a maintainer or collaborator ruled out. A requirement scoped to PRs or commits, not comments, doesn't apply here. If the repo states no requirement for comments and the thread rules nothing out, it passes. | required |
| fit | The plan comment, read against the thread highlights | Passes if the comment engages the thread's direction: it builds on maintainer suggestions, acknowledges related issues or PRs, and reads as a response to this thread, not a generic claim. | preferred |

## Verdict rule

Accept (ready) if every required check passes. Any required check that fails holds the package. Preferred checks never change the verdict. Unclear counts as fail.
