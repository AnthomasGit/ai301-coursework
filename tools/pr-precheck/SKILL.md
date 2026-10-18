---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to decide whether it is ready to submit.
Do not answer from gut feel, and do not invent a workflow. Execute the
student-authored procedure in `procedure.md`, which applies the rubric
in `rubric.md` to evidence you locate with
`references/evidence-guide.md`. This file tells you what you read,
when you refuse to grade, and what you emit.

## The question

You answer one question about one PR package: **is this ready to
submit?**

A PR package is a candidate pull request, made up of:

- its title
- its description
- its commits
- its diff
- its test evidence

Read the package against two things:

- **the plan it claims to implement.** This includes any deviation
  notes the plan records.
- **the issue that plan belongs to.** This includes the thread and the
  repo's stated asks.

Grade exactly one package per run. Do not answer a different question,
such as:

- whether the code is good in general
- whether a maintainer should merge it
- whether the issue was worth fixing
- whether the plan was a good plan

The plan was graded last week. This week you read it as the yardstick
the PR is held to.

## Inputs and modes

You run in exactly one of two modes per run. Decide which one first.

### Live mode

Live mode checks the student's own draft PR before it goes out. The
student invokes it from their Path Review working copy with a command
of this shape:

    pr-precheck: grade my draft PR for issue <URL>: plan in plan.md,
    the diff on this branch, title and description in pr_draft.md,
    test evidence in test_evidence.md

Read exactly these inputs:

- **The plan:** `plan.md` at the working copy root. This is the posted
  plan, with any deviation notes it records. A deviation note is part
  of the plan, and it is graded like the rest of it.
- **The diff:** the output of `git diff main...HEAD` (three dots), run
  from the working copy. This is everything the branch changes
  relative to the repo's default branch, and it is the only source for
  the diff.
  - Do not substitute uncommitted working-tree changes.
  - Do not substitute a PR page.
  - Do not substitute a two-dot diff.
- **The commits:** `git log main..HEAD` on the same branch.
- **The draft title and description:** `pr_draft.md`.
- **The test evidence:** `test_evidence.md`. This is the captured test
  output, including the outcome of the repo's own checks or suite.
- **The issue thread:** the issue at the URL the student gave, fetched
  live with `gh`, the GitHub API, or the web. Read the body, every
  comment, and any explicit maintainer direction.
- **The repo's stated asks:**
  - its PR template, `.github/PULL_REQUEST_TEMPLATE.md`
  - its contributing guide, `docs/CONTRIBUTING.md`, including any
    stated policy such as AI-use disclosure

**House-chain students.** A student on the house chain reads the house
plan and the house repro pack in place of their own plan and repro
report. The same checks grade the same things there.

**The package is what these inputs contain, and nothing else.** Do not
treat other files in the working copy (for example a leftover
`comment.md` or `evidence.md`) as part of the package. Read the drafts
the way a maintainer will read the opened PR.

**Refuse to grade, and say which input is missing, if:**

- `plan.md`, `pr_draft.md`, or `test_evidence.md` does not exist or is
  empty
- `git diff main...HEAD` fails or produces no diff
- the issue URL is missing or cannot be read

A missing input is not a failing check to grade around. It means there
is no complete package yet.

### Eval mode

Eval mode grades a practice submission. That is a package bundle, one
of the frozen PRs in the clone's `eval/packages/` folder. It is a
markdown file with these sections:

- repo facts
- the issue
- thread highlights
- plan context (the accepted plan and the repro evidence it built on)
- the candidate PR's title, description, commits, diff, and test
  evidence

The bundle is the whole world:

- Use only the bundle text as evidence.
- Fetch nothing.
- Read nothing outside the bundle, including any repo, `scope.md`, or
  `voice-guide.md`.

Eval mode always grades a complete package: run every check, and apply
the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else, before any other file and before running any git command.

1. **Check the placeholder.** If the `Repo:` line still carries an
   unfilled placeholder, stop without grading. Tell the student to
   fill the `Repo:` line in `scope.md` with their section's Path
   Review repo. Never guess a scope.
2. **Check the repo.** Confirm that the issue URL points into the
   scoped repo, and that the working copy's upstream remote does too.
   If either points anywhere else, refuse to grade. Say the PR is
   outside this tool's scope and name the scoped repo.
3. **Apply the house rules** as how evidence is read in that room:
   - The PR comes from a `fix/<issue-number>-<slug>` branch on the
     student's fork. The number in the branch name must match the
     issue's number.
   - There is one bounded PR per issue, delivering the posted plan.
   - The repo's PR template is always used, with every section given
     real content, including the disclosure section where it applies.
   - A classmate's PR on the same issue is not a blocker, and it is
     not a reason to fail any check.
   - Credit attaches to opening the PR, so never grade what staff
     might do after it is opened.

In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the scope check passes, also read `voice-guide.md`.
These are the student's own rules for how they write upstream, with
wrong and right examples.

- Hold the outgoing PR text against those rules. The outgoing text is
  the title and the description in `pr_draft.md`.
- In the summary, report every rule the draft breaks. Quote the rule
  and the offending text.
- The voice guide never changes the verdict on its own unless a check
  in `rubric.md` reads it.

In eval mode, ignore `voice-guide.md` entirely. Voice is personal and
carries no gold labels. The universal communication-quality checks
live in the rubric.

## Component reads

The tool is four components, and you read all four before grading:

1. **`rubric.md` decides what.** It holds:
   - A table of checks. Each row names the check, the evidence to
     gather, the pass condition, and a weight. `required` checks gate
     the verdict, and `preferred` checks never change it.
   - The verdict rule that combines check grades into `accept` or
     `reject`.
2. **`references/evidence-guide.md` is the map.** For each evidence
   family, it says where the evidence lives in a bundle and, in live
   mode, in the working copy and on GitHub. It also says what good
   looks like there. There are four families, and they line up with
   the harness's failure categories:
   - plan fidelity (silent drift)
   - test evidence (not tested)
   - diff quality (unreviewable)
   - standards and comms (standards wall)
3. **`procedure.md` decides how.** Execute it as written:
   - its read order
   - its evidence gathering
   - its check execution
   - its verdict assembly

   Follow it exactly, without improvising around gaps. Where the
   procedure is silent on a step, note the gap in your summary rather
   than inventing a step. A procedure gap is feedback the student
   needs.
4. **`scope.md` and `voice-guide.md`** apply in live mode only, as
   described above.

**The refusal rule.** If `rubric.md` has no checks filled in, or
`procedure.md` has no steps filled in, stop and say so. Grade nothing,
and do not invent checks or steps at runtime. HTML comments and
template guidance do not count as content; only the student's own
written text does. This tool cannot grade without a rubric and a
procedure, and that is by design.

## Verdict and output

The verdict space is binary:

- `accept` means ready to submit.
- `reject` means hold.

There is no third verdict, no "accept with reservations", and no
score. Reservations belong in a check's evidence line, not in the
verdict.

Before the JSON block, you may show a short readable summary. Include:

- one line per check
- the deciding check, as `procedure.md` picks it
- in live mode, any voice-guide notes
- any procedure gaps you hit

Then end your reply with this fenced JSON block, and put nothing after
it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

Fill in the block's fields this way:

- **`item`:** in eval mode, use the bundle id. In live mode, use the PR
  URL if one exists; for a draft that has not been opened yet, use the
  issue URL.
- **`checks`:** one entry per rubric check, in rubric order, using the
  rubric's check names.
- **`evidence`:** one line.

The eval harness parses the last fenced JSON block in your output, so
it must be present, valid, and last. When you refuse to grade (a scope
stop, a missing live input, or an empty rubric or procedure), say why
in plain text and emit no verdict.

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it, such as a file in the diff, a line of test
  output, or a sentence of the plan. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be
  ready. A beautiful, confident one can hide drift. Read the diff
  against the plan, the evidence against the test plan, and the
  description against the diff. Never grade the formatting, the
  length, or the number of commits.
- **Honest outcomes can be ready.** A PR that honestly discloses a
  shortfall or a recorded deviation can be ready. "Less than
  everything" is not by itself a reason to hold. A deviation that
  exists only in the diff is silent drift.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you want; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's verdict
  rule directs. Where the rule is silent, treat `unclear` as `fail`. A
  PR you cannot verify from the package is a PR that is not ready to
  submit.
