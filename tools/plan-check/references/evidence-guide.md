# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

Used by the **diagnosis** check.

- **Where it lives (eval):** the plan's explanation of the cause is in
  its *Diagnosis* section, and the fix is in its *Changes* section. The
  behavior that the cause has to explain is in the *Repro evidence*
  block: each step, what was changed in that step, and what happened.
- **Where it lives (live):** the cause is in the Diagnosis section of
  the draft `plan.md`. The behavior is in the student's own repro
  comment posted on the issue (or, on the house issue, the house repro
  pack as the drafts quote it).
- **What good looks like:** the stated cause explains every result in
  the repro evidence, including steps where the problem stayed the same
  or went away, and no step contradicts it. The planned change fixes
  that cause directly, not just the place where the problem becomes
  visible.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

Used by the **scope** check.

- **Where it lives (eval):** the plan's *Scope* section (what it says
  it will change and what it will leave alone), compared with where
  the repro evidence shows the cause to be.
- **Where it lives (live):** the Scope section of `plan.md`, compared
  with the student's posted repro comment.
- **What good looks like:** it names the files or areas it will change
  and says what it won't touch. The area it will change includes the
  place the repro evidence points to, and nothing the evidence points
  to is left out. The change stays limited to that area, with no extra
  refactoring or unrelated fixes.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

Used by the **approach** check.

- **Where it lives (eval):** the plan's *Changes* steps, compared with
  its *Scope* section.
- **Where it lives (live):** the Changes or Approach section of
  `plan.md`.
- **What good looks like:** each step names a specific file or function
  and a specific action, listed in the order the work will happen, so
  someone new to the project could start the work without asking the
  author any questions. Every step stays inside the stated scope.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

Used by the **test** check.

- **Where it lives (eval):** the plan's *Test plan* section, plus any
  test listed in *Changes*, compared with the steps in the repro
  evidence.
- **Where it lives (live):** the Test plan section of `plan.md`, plus
  the repo's test folder (for example `tests/`) if the plan says where
  the test will go.
- **What good looks like:** an automated test that repeats one of the
  repro steps (the same input and conditions) and checks a result you
  can observe, such as a time limit, an output, or an exit code. The
  test fails on the current code and passes once the fix is in.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

Used by the **unknowns** check.

- **Where it lives (eval):** every factual claim in the plan and the
  plan comment, checked against the *Repro evidence* and *Repo facts*
  blocks. Any stated unknowns are in a Risks, Assumptions, or Open
  questions section, if the plan has one.
- **Where it lives (live):** the same, in `plan.md` and the draft
  comment. If the build later departs from the plan, the change is
  recorded as a dated note in `plan.md` saying what changed and why.
- **What good looks like:** anything the evidence doesn't prove is
  labeled as an assumption or open question, ideally with how it will
  be checked. Ideas taken from other people's comments are presented
  as their suggestions, not as settled fact. Confident words such as
  "traced", "identified", or "the cause is" are always backed by a
  result from the repro evidence.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

Used by the **fit** check.

- **Where it lives (eval):** the *Candidate plan comment*, compared
  with the *Thread highlights* (especially what maintainers said) and
  the *Repo facts* block (issue templates, contribution guide, AI
  policy).
- **Where it lives (live):** the draft plan comment, compared with the
  issue's discussion on GitHub and with the repo's `CONTRIBUTING.md`,
  issue templates, and any rule about disclosing AI use.
- **What good looks like:** the comment responds to what maintainers
  have said (it follows their direction, or explains its approach in
  light of it), doesn't propose anything they have already ruled out,
  follows any required format or disclosure, and talks about the
  specifics of this issue rather than saying something that could be
  posted anywhere.
