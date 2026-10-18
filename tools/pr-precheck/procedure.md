# Procedure: how this tool grades a PR package

## Read order

Read the plan before anything the PR says about itself. Then read the
diff before the description. The plan is the yardstick, and the diff
is what actually ships. If you read the description first, its claims
("implements the plan exactly", "no options added") shape how you read
the diff. That is the trap the side-by-side checks exist to catch.

In live mode, `scope.md` and `voice-guide.md` are already read per
`SKILL.md`. Take each part below from the live source that
`SKILL.md` names: `plan.md`, `git diff main...HEAD`,
`git log main..HEAD`, `pr_draft.md`, `test_evidence.md`, the issue
thread, the PR template, and `docs/CONTRIBUTING.md`. In eval mode,
take each part from the bundle section with the same name.

1. **Repo facts** (live: the PR template and `docs/CONTRIBUTING.md`).
   Write the **asks list**: every mandatory ask for a PR, one line each.
   - each required template section
   - each required checklist item
   - the issue-link form (for example `closes #N`)
   - each required artifact (for example a changelog entry, a whatsnew
     entry, or docs)
   - any AI-use disclosure rule, with what it must state

   Mark soft wording ("keep PRs small") as soft. Only mandatory asks
   go on the list.
2. **Issue.** Note the reported symptom and the expected behavior, one
   line each.
3. **Thread highlights** (live: the full thread). List each explicit
   maintainer direction, question, or decision as
   *author (role): ask*.
4. **Plan context** (live: `plan.md`). Write the **plan sheet**:
   - **Files:** every file, folder, or described artifact the plan
     names (for example "a changelog entry").
   - **Promised changes:** each change the plan says it makes, one
     line each.
   - **Out of scope:** the not-in-scope line, quoted.
   - **Notes:** each deviation or deferral note, quoted. Live, this
     includes deviation notes added to `plan.md` after posting.
   - **Test plan:** each item, tagged **fix** (a failure mode the fix
     must remove), **control** (a run that should stay unchanged), or
     **suite** (the repo's tests or checks).
   - **Repro step:** the failing step from the repro evidence, quoted.
5. **Diff** (live: `git diff main...HEAD`). Write the **hunk list**:
   one line per hunk, giving the file, what the hunk does in a few
   words, and whether it adds or removes behavior or only reshapes
   code.
6. **Commits** (live: `git log main..HEAD`). List each message.
7. **Title and description** (live: `pr_draft.md`). Read them in full,
   checklist included.
8. **Test evidence** (live: `test_evidence.md`). Read it in full.

## Evidence gathering

Build three side-by-side tables, then fill the per-check record. Each
table pairs one thing the PR delivers with the thing it is held to.
Write every row down; do not grade in your head.

1. **Diff against plan (the file and hunk table).** For each entry in
   the hunk list, record two things:
   - the plan-sheet line that covers it: a Files entry, a Promised
     change, or a Note
   - whether it collides with the Out of scope line

   Mark a hunk **unplanned** if no plan-sheet line covers it. Then go
   the other way: for each Promised change, record the hunk that
   delivers it, or the Note that defers it, or **missing**. A file
   counts as covered when the plan names the file, its folder, or a
   description that picks out exactly that file.
2. **Claims against diff (the claim ledger).** List every concrete
   claim in the title, description, and checklist:
   - fidelity claims ("exactly as planned", "no changes beyond it")
   - "added" claims (tests for named cases, a changelog, docs)
   - "not added" claims ("no options added", "no API change")
   - "n/a" answers

   Tag each one with the hunk that proves it. A "not added" claim is
   proved when no hunk adds the thing; search the hunk list for it. For a
   claim of tests for N cases, find each case's input in the added
   tests or fixtures. An assertion inside a shared test function
   counts, so check the assertions, not only the function count. Tag any claim with no proving hunk **unproved**. Tag a
   fidelity claim **contradicted** if the file and hunk table has any
   unplanned hunk or out-of-scope collision.
3. **Evidence against test plan (the test table).** For each Test plan
   item, record the evidence block that covers it, if any, and what
   that block shows:
   - **fix** items: for the repro's failing step, a before line and an
     after line. For any other fix item, at least an after, which may
     be a sentence naming the observed result. Mark a fix item the
     evidence never mentions **absent**. For each after, record whether the
     after states an observed result (output, value, error text, exit
     code, or the named error's absence) or only a verdict ("works",
     "matches expected byte for byte", "verified"). Also record which
     input the run used, and whether that input goes through a path the
     diff changes or only through a control or an unchanged path.
   - **control** and **suite** items: whether there is a command plus
     its printed output or result line, or only a sentence or a `#`
     note saying it passed.

   A fix item that a Note defers is marked **deferred**.
4. **Fill the per-check record** from the tables and the read notes:

| Check | Pull from | Record |
|---|---|---|
| plan-fidelity | The file and hunk table | Each unplanned hunk (file plus what it does), each out-of-scope collision, and each Promised change marked missing. Or "every hunk covered, every promise delivered or deferred". |
| claims-match-diff | The claim ledger | Each unproved or contradicted claim, quoted, with what the diff shows instead. Or "every claim proved". |
| repro-evidence | The test table's fix items, read against the plan sheet's Repro step | For each fix item that is not deferred: the after line quoted, whether it is an observed result or a verdict, and whether its input goes through the changed path. |
| clean-diff | The hunk list, read line by line | Each debris line, with its file: a debug print or log, commented-out code, a dead or unused function, a TODO, whitespace, re-indent, or import churn, or a restyle of code the fix does not need. Unplanned refactor hunks from the file and hunk table count here too. |
| repo-asks | The asks list, read against the description and the hunk list | For each mandatory ask: met (quote where), "n/a" with a reason the diff bears out, or unmet. For a disclosure rule, quote the disclosure or record that it is absent. |
| checks-shown | The test table's control and suite items, and the commit list | Each control or suite item shown only as a claim, and each placeholder commit message ("wip", "fix", "misc"). |
| thread-fit | The thread notes, read against the description | Each maintainer direction or question, and whether the description engages it (builds on it, answers it, or cites it for a deferral). |

5. Live, take each family from the locations in
   `references/evidence-guide.md`. Eval, use only the bundle text. A
   live input that is missing stops the run per `SKILL.md`; do not
   gather around it.

## Check execution

1. Run the checks in rubric order: plan-fidelity, claims-match-diff,
   repro-evidence, clean-diff, repo-asks, checks-shown, thread-fit.
   Plan-fidelity goes first because claims-match-diff reads its result
   for fidelity claims.
2. Grade every check, even after a required check has failed. The
   output lists all seven.
3. Grade each check by applying its pass condition from `rubric.md` to
   the evidence recorded for it, and nothing else.
   - **P** (pass): the record meets the pass condition.
   - **F** (fail): the record breaks the pass condition, **or** the
     package lacks something the condition requires. Some examples: no
     before/after for a fix item, a promised test with no fixture, a
     required changelog entry absent from the diff. Something missing
     is a fail, not an unclear.
   - **?** (unclear): the package addresses the point, but its text
     cannot decide it. Examples: an after line that might be output or
     might be a paraphrase, or a hunk whose purpose is not visible in
     the diff and might be needed by the fix.
4. Each grade names one deciding fact: a hunk (file plus line), a
   quote from the description or test evidence, or an asks-list line.
   If you cannot name one, re-read only that check's Pull-from source,
   not the whole package.
5. For repro-evidence, a fix item other than the repro's failing step
   that the evidence covers with one sentence naming the observed
   result (for example "same-key redefine prints the one-time
   warning") grades P for that item, not ?. Only the repro's failing
   step needs a shown before and after.
6. Honest disclosures are read as written. A change or fix item that a
   plan Note or the description explicitly defers is not missing, and
   it is not untested. A disclosure is not a failure in itself.
7. The pass condition decides, not your impression. A polished
   description over a drifting diff still fails plan-fidelity. A terse
   description over a clean, matched, evidenced diff still passes. If
   a grade feels wrong, keep it and note the tension for the summary.

## Verdict assembly

1. Collect the grades of the required checks: plan-fidelity,
   claims-match-diff, repro-evidence, clean-diff, repo-asks. Count each
   **?** as **F**.
2. If every required check is P, the verdict is **accept**. If any is
   F, the verdict is **reject**.
3. The preferred checks are checks-shown and thread-fit. Report their
   grades, and list their fails and unclears as fixes to make before
   submitting. They never change the verdict.
4. The deciding check is the first required check, in rubric order,
   that is not P. Quote its deciding fact in the summary. When several
   required checks fail, quote only the first; the others appear on
   their own lines. For an accept, quote repro-evidence's deciding
   fact.
5. Write the short summary:
   - one line per check
   - the deciding check
   - any rubric tensions noted while grading
   - any gaps in this procedure you hit
   - in live mode, the voice-guide notes

   Then emit the JSON block from `SKILL.md`. Give each check's grade as
   `pass`, `fail`, or `unclear` (as graded, before ? counts as F), with
   its deciding fact as a one-line `evidence`. Set `verdict`. Put
   nothing after the JSON block.
