# Evidence guide: where evidence lives in a PR package

This guide is the map for each evidence family. Each family has three
parts:

- **In a practice submission:** the eval bundle section that holds the
  evidence.
- **In your own PR:** the live file or command that holds it.
- **What good looks like:** a condition someone else could apply.

The worked example throughout is `calib-01`, a clear accept: a
one-line theme fix that matches a two-step plan.

## Plan fidelity (harness category: silent-drift)

**In a practice submission:**

- **What the plan covers:** the `## Plan context` section. The files
  it names follow `Files:`. The planned change is in the `Plan:`
  paragraph (often "Scope, one bounded change: ..."). The exclusions
  follow `Not in scope:`. Any deviation or deferral note appears there
  too (for example "Deliberately deferred" or "Known limit").
- **What actually changed:** `### Diff` under `### Candidate PR`. Every
  `--- a/` / `+++ b/` header names one changed file, and every `@@`
  hunk under it is one change.
- **What the PR claims about fidelity:** `### Title` and
  `### Description`, including any checklist or "Requirements" lines
  inside the description.

**In your own PR:**

- **The plan:** `plan.md` at your working copy root, read with its
  `Files`, scope, not-in-scope, and test-plan parts. Read any
  deviation notes you added after posting the plan as part of it.
- **The diff:** `git diff main...HEAD` (three dots), run in the
  working copy. `git diff --stat main...HEAD` lists the changed files
  to tick against the plan.
- **The claims:** the title and description in `pr_draft.md`.

**What good looks like:**

- Every changed file is named in the plan: the file itself, its
  folder, or a description that picks out exactly that file (for
  example "a changelog entry").
- Every hunk does something the plan promised.
- Nothing in the diff touches what `Not in scope` excludes.
- Every promised change is in the diff, or a deferral note in the plan
  or the description names the part left out.
- Every concrete claim in the description is borne out by a diff
  line. That includes "exactly as planned", "no options added", and
  "tests for X, Y, Z".

calib-01 meets all of these. The plan names `themes.gitconfig` and one
removal. The diff touches only `themes.gitconfig` and removes exactly
the shadowed `#434C5E` line. The description claims nothing the diff
does not show.

**What silent drift looks like:**

- **More than the plan:** an extra file, a refactor, or a new
  option or flag with no deviation note. Example: calib-03's
  `print/property.js` rewrite under a description that says "no
  changes beyond it".
- **Less than the plan:** a promised piece missing with no note, or
  the description claiming a piece the diff doesn't contain. Example:
  pkg-17's docs.

## Test evidence (harness category: not-tested)

**In a practice submission:**

- **What the PR must prove:** the `Test plan:` sentence in
  `## Plan context`, plus the failing step quoted under
  `Repro evidence`.
- **The proof:** `### Test evidence`, and any before/after blocks in
  `### Description`.
- **Tests the PR adds:** test files in `### Diff`.

**In your own PR:**

- **What it must prove:** the test plan in `plan.md`, plus your
  repro's failing step (or the house repro pack, for the house chain).
- **The proof:** `test_evidence.md`, the before/after runs and the
  repo's own test or check output you captured in step 8.
- **Tests you added:** the test files in `git diff main...HEAD`.

**What good looks like:**

- The repro's failing step is re-run with a before and an after. If
  the test plan names more failure modes, each one is shown at least
  as an after naming the observed result (a sentence is enough), unless
  a note defers it. A failure mode the evidence never mentions is
  untested.
- The after states what was observed: the printed output, a value,
  the error text, an exit code, or the named error being gone.
- The run goes through the code the diff changed, not only through a
  control path.
- The repo's own suite or checks are run by name, ideally with a
  result line.

calib-01 meets these. The before line is the
`DuplicateOptionError: ... already exists`, the after is the printed
`parsed OK`, and `git config --get-all` is shown printing exactly one
value.

**What not-tested looks like:**

- A verdict in place of the result: "tests pass", "works now",
  "verified on my machine", or "matches the expected output byte for
  byte" with no output shown.
- Only one of two failure modes re-run (calib-04).
- The evidence runs an input that never reaches the changed path
  (pkg-07, pkg-14).

A control or suite run that is only claimed ("control unchanged",
"`go test` passes") is a weaker fact. Record it for the preferred
check `checks-shown`.

## Diff quality (harness category: unreviewable)

**In a practice submission:** `### Diff`, read hunk by hunk and line
by line, and `### Commits`, one message per commit.

**In your own PR:** `git diff main...HEAD` and
`git log --oneline main..HEAD`. Check these before you open the PR:

- Uncommitted scratch files don't count.
- Committed debug lines and experiments do count.

**What good looks like:**

- Every `+` or `-` line is part of the fix or its test.
- Unchanged context lines around a hunk are fine.
- An import or fixture the fix itself needs is fine.
- Each commit message says what that commit changes.

calib-01 is one hunk with a one-line deletion and one descriptive
commit (`themes: remove shadowed file-decoration-style from
gruvmax-fang`).

**What unreviewable looks like:** debris riding with the fix.

- a debug print or log line
- commented-out code or a commented-out first attempt
- a dead or unused function (for example one behind
  `allow(dead_code)`)
- a TODO left on the change
- re-indent, whitespace, or import-order churn
- identical lines re-printed
- a restyle of code the fix does not need
- `wip` / `fix` / `misc cleanups` commit messages (pkg-12, pkg-15,
  pkg-18)

## Standards and comms (harness category: standards-wall)

**In a practice submission:**

- **The repo's asks:** `## Repo facts`. The `pull requests:` line
  gives the template sections, checklist items, issue-link form, and
  required artifacts such as a changelog or whatsnew entry. The
  `contribution policy:` line gives the stated AI policy.
- **Maintainer direction:** `## Thread highlights`.
- **Where the PR honors the asks:** `### Description` holds the
  sections, checklist answers, `closes #N`, and any disclosure.
  `### Diff` holds required artifact files such as
  `changelog_unreleased/...` or `CHANGELOG.md`.

**In your own PR:**

- **The asks:**
  - `.github/PULL_REQUEST_TEMPLATE.md`, for every section and
    checklist item
  - `docs/CONTRIBUTING.md`, for the stated rules and any AI-use
    disclosure rule
  - the issue thread on GitHub, for maintainer direction
- **Where the PR honors them:** `pr_draft.md` from step 9 holds every
  template section filled and the disclosure. `git diff main...HEAD`
  holds any required artifact.
- **Path Review house rules** (from `scope.md`):
  - the template is always used, with every section filled
  - the branch is `fix/<issue-number>-<slug>`

**What good looks like:**

- Every mandatory ask is met with real content.
- Each required section has text specific to this change.
- Each checklist item is answered. An "n/a" needs a reason that is
  true of the diff.
- The issue is linked in the form the repo asks for.
- Required artifacts are in the diff.
- Where the policy requires AI-use disclosure, the description states
  the tool and the extent, in the PR author's own words.
- If the repo states no asks, there is nothing to meet.

calib-01's repo has no template and no AI policy. It asks only that
PRs stay small and reference the issue. The one-hunk diff and
"(#2211)" in the description meet both asks.

**What a standards wall looks like:**

- a required checklist skipped
- `closes #N` missing
- a required whatsnew entry absent from the diff (pkg-01)
- the stated AI policy unmet, with no disclosure in the description
  (pkg-20)

Whether the description's claims match the diff is plan fidelity
(above), not this family.
