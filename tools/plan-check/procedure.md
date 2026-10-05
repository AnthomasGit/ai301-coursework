# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

Read the package in this order. Read the repro evidence before the
candidate plan, so that the plan's explanation can't shape how you
read the evidence. The diagnosis, scope, and unknowns checks all rely
on that independent reading.

1. **Repo facts.** Note the bug-report template's asks, the
   contribution guide and any AI-use policy, and the latest release.
2. **Issue.** Note three things, one line each: the reported symptom,
   the expected behavior, and the environment.
3. **Thread highlights.** List each comment as *author (role): claim
   or ask*. Mark direction from a maintainer or collaborator (for
   example, a pointer to another issue or a "known problem" statement).
   Mark any root-cause claim made by a non-maintainer as
   **unverified**, unless the repro evidence later confirms it.
4. **Repro evidence.** Write a **pinned-facts list**: for each step,
   the variable that changed and what happened to the symptom (for
   example, "feature turned off: problem went away"). Then write one sentence
   saying where these facts put the cause: which component, which
   variable it tracks, and which components it rules out.
5. **Candidate plan**, in full.
6. **Candidate plan comment**, in full.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. **Build the claim ledger.** List every factual claim made in the
   plan (Summary, Diagnosis, Scope, Changes, Test plan) and in the
   plan comment. Tag each one:
   - **E** (established): a pinned fact or a repo fact supports it, or
     it is a code-level explanation that is consistent with the pinned
     facts and sits inside the component they localize.
   - **C** (contradicted): a pinned fact contradicts it. Note which
     one.
   - **U** (unverified): nothing in the package settles it either way.

   For each claim, also record its source: whether the plan cites the
   thread, cites the repro, or just asserts it.
2. **Gather the evidence for each check** from the package location
   below, and record what is listed:

| Check | Pull from | Record |
|---|---|---|
| diagnosis | The plan's Diagnosis and Changes sections, and the pinned-facts list | The stated cause as a quote, with its ledger tag. Whether it sits in the component and trigger the pinned-facts sentence names. Whether the planned change acts on that cause or only on where the symptom shows up. |
| scope | The plan's Scope section, and the pinned-facts sentence on where the cause sits | Every file or area named as in scope. The not-in-scope line as a quote. Whether anything excluded is the area where the pinned facts place the cause (an F), or only a related variant or secondary factor deferred with a reason (not an F). |
| approach | The plan's Changes steps, compared with the Scope section | For each step: whether a stranger could carry it out without asking the author, and whether it stays inside the stated scope. |
| test | The plan's Test plan, compared with the repro steps | Which repro step the verification re-runs, if any. Whether it is automated or a stated re-run with an expected outcome. Whether it would fail on the current code and pass once the fix is in. |
| unknowns | The claim ledger, filtered to load-bearing claims (the cause, and that the change removes the symptom) | Each load-bearing claim tagged C or U, and whether the plan marks it as an assumption or open question or presents it as fact. Ignore U-tagged implementation details. |
| conventions | The repo facts' contribution policy, the thread notes, and the plan comment | Each mandatory requirement, and whether it applies to issue comments or only to PRs or commits. For each one that applies to comments, whether the comment meets it. Each approach a maintainer or collaborator ruled out, and whether the comment proposes it. |
| fit | The plan comment, compared with the thread notes | Each maintainer or collaborator suggestion, related issue, or PR, and whether the comment engages it. |

3. In live mode, take each family from the locations in
   `references/evidence-guide.md`. In eval mode, use only the bundle
   text.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Run the checks in rubric order: diagnosis, scope, approach, test,
   unknowns, conventions, fit. Diagnosis goes first because approach and test are
   judged partly against the cause.
2. Grade every check, even after a required check has failed. For
   scope, excluding the area where the pinned facts place the cause is an F. The
   output lists all of them.
3. Grade each check by applying its pass condition from `rubric.md` to
   the evidence recorded for it. Nothing else counts.
   - **P** (pass): the recorded evidence meets the pass condition.
   - **F** (fail): the recorded evidence breaks the pass condition,
     **or** the plan is missing something the condition requires (for
     example, no not-in-scope line, or no verification that re-runs the repro). Something
     missing is a fail, not an unclear.
   - **?** (unclear): the plan addresses the point, but the package
     can't decide it either way (for example, a cause outside the
     component the pinned facts localize, that they neither support nor
     contradict). A cause inside that component and consistent with the
     pinned facts is a P, not a ?.
4. Each grade names one deciding fact: a quote from the package, or a
   ledger entry. If you can't name one, re-read only the package
   section listed for that check, not the whole package.
5. The pass condition decides, not your impression. If a check passes
   by the rubric's wording but seems wrong, grade it P and record the
   tension for the summary.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Collect the grades of the required checks: diagnosis, scope,
   approach, test, unknowns, conventions. Count each **?** as **F**.
2. If every required check is P, the verdict is **accept**. If any
   required check is F, the verdict is **reject**.
3. Fit is preferred: report its grade, but it never changes the
   verdict.
4. The deciding check is the first failed required check in rubric
   order, or "all required checks passed" for an accept. Quote its
   deciding fact in the summary.
5. Write the short summary: one line per check, plus any rubric
   tensions noted while grading checks. Then emit the JSON block from
   `SKILL.md`, with `pass|fail|unclear` grades and each check's
   deciding fact as its `evidence` line. Put nothing after the JSON
   block.
