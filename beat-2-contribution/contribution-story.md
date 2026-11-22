# Contribution: Parser: a call in the base list (`ExportModelOperationsMixin("food")`) drops the model

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Thomas Longobardi
**GitHub Username:** AnthomasGit
**Issue:** https://github.com/FROWNINGdev/orm-lens/issues/335
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Wrote issue-scout's search strategy and patched it and the rubric across nine live scans (2026-11-22)**

I wrote seven `gh search issues` queries, each traced to a line of my fit
profile, plus ranking filters and repo tiers, and ran the scout against all of
GitHub. The first scan accepted two issues in a 2-star Jenkins plugin, one
already claimed in a comment, and later scans kept spending grading slots on
known dead ends: issues opened years ago (the queries filtered on last update,
the rubric on open date), claims written into the issue body, unlinked fix
PRs, and features far bigger than 2–6 hours. I added `--created` to every
query, dropped the beginner-label requirement on my starting-point repos
(those repos had 0 labelled issues from the last 90 days but 43 overall),
reordered the ranking, kept a list of already-graded rejects, and added rubric
checks for comment and body claims, repo adoption, AI policy, bounded scope,
contributor-doable work and a 2–6 hour size, logging each change in
`scan-notes.md`. Nine scans still produced no accepted issue that fits my
profile, so the real limit is supply; the one accept that held up,
orm-lens#335 from scan 3, passed again when I re-graded it directly under the
final rubric.

**Unit 6: Installed the voice guard with upstream rules in my voice guide, and wrote orm-lens's repo memory file (2026-11-22)**

I folded my three voice-guide copies into one canonical guide, added five
upstream rules for claims, questions and pings, and hooked the guard into
Claude Code; it stopped the wiring test and passed my claim before it went up
on orm-lens#335. Two things fell short: Windows refused the symlinks, so each
skill's `voice-guide.md` is now a one-line pointer to the canonical file, and
the hook only watches Bash, so when the wiring test reached for PowerShell
first the guard never saw it, and I now post only through Bash with
`--body-file`. Held against my own rules, my old claim draft promised a PR
"within about a week", so I cut the timeframe and made the report-back
"before I start on the fix". For orm-lens I wrote a 59-line `CLAUDE.local.md`
from one scoped read of the parser and its tests, with the pytest, ruff and
mypy commands checked by running them (1310 passed), kept out of my PR through
`.git/info/exclude`; the TypeScript tests are not run yet.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I ran my scout against all of GitHub with seven, later nine, `gh search issues`
queries, each tied to a line of my fit profile, and graded the top five of
each scan against my rubric. Across nine scans that was about 45 graded issues,
mostly in my starting-point repos (opentelemetry-python and -contrib, the
Prometheus and Grafana Helm charts, splunk-sdk-python, a jenkinsci plugin) and
a few small repos. FROWNINGdev/orm-lens#335 surfaced in scan 3 from Q1, my
broad Python good-first-issue query. It is a parser bug: a call in a class's
base list, like django-prometheus's `ExportModelOperationsMixin("recipe")`,
makes orm-lens skip the whole Django model. The scout accepted it in scan 3,
again when I graded it directly under the full rubric, and a third time in
this week's re-grade after the scope patch, with the difficulty note
"straightforward": the issue names the file and the lines, suggests the regex
and loop, and lists the tests. I re-graded last week's two picks the same day,
jenkinsci/batch-control-plugin#150 and #152, and both now reject: each has a
claim comment from someone else, and #150 already has an open PR.

The fit is honest but partial. It is a small Python bug fix with a repro, a
named cause and a test plan, well inside my 2-6 hours, in a repo with 114 stars
and active maintainers. The project area is not ops tooling, though: it is
static analysis for the Django ORM, and the only link to my profile is that the
trigger is a Prometheus mixin. I chose it anyway because it is the one accepted
issue that held up after every rubric change, and nothing in my tier-1 repos
survived grading. From it I want to learn how a real project's review works end
to end, how to fix a parser bug without breaking its other callers, and whether
the same fix belongs in the TypeScript parser under the repo's parity rule,
which is the question I asked in my claim. My read of the scout: the search
side worked, but the rubric was built on the sandbox's tidy issues. Real issues
kept failing on claims written into the issue body, unlinked fix PRs and
unsettled design, and nine scans produced no accepted issue that also fits my
profile well. The limit I hit is supply in my own area more than the tool.

Claim comment: https://github.com/FROWNINGdev/orm-lens/issues/335#issuecomment-7085240055

**Toolkit**

- Tool: issue-scout, with my search strategy and rubric, run as nine live
  scans and then a direct re-grade of my three picks
  (`issue-scout: grade <url> <url> <url>`)
- What it produced: nine ranked scan read-outs; orm-lens#335 accepted in scan
  3 and in both direct grades, with the difficulty note "straightforward"; the
  re-grade rejected batch-control-plugin #150 and #152 on Not Claimed (and
  Repo Adoption at the time)
- Difficulties: my Bounded Scope check and Fits 2-6 Hours failed any thread
  with an unsettled design, which most real issues have; scans 8 and 9
  accepted none of ten graded issues, on claims, holds and open designs; the
  25-star floor rejected every new jenkinsci plugin, though Jenkins is on
  my tool list
- What changed: applied the unit-6 scope patch (Bounded Scope replaced by
  `scope-feasible`, plus a difficulty note that never changes the verdict),
  stopped Fits 2-6 Hours from failing an unfinished design, and let Repo
  Adoption pass repos owned by the orgs my strategy targets; each change is a
  dated line in `scan-notes.md`

---

## Understanding the Issue

**Problem Description**

orm-lens finds Django models by reading `models.py` files with regexes
instead of importing them. Its class-header regex can't handle a call inside
the base list, so a model like
`class Comment(ExportModelOperationsMixin("comment"), models.Model):` is
skipped without any message. That pattern is common: django-prometheus
exports per-model metrics through exactly this mixin factory. On Tandoor
Recipes, the issue reports, 22 of 48 models disappear this way, including
central ones like `Recipe`, and `drift` then reports their tables as stale.

**Expected Behavior**

A call in the base list is ignored and the other bases decide whether the
class is a model. On the issue's repro, `orm-lens list -v` reports
`found 1 apps / 2 models` with `shop.Comment` and `shop.Plain`, and
`Comment` keeps its `text` field. A class whose only base is a call, like
`class Plain(SomeFactory("x")):`, is still not a model, and a header wrapped
over several lines works the same as a one-line one.

**Current Behavior**

On `main` at `ac40f8d` the repro gives `found 1 apps / 1 models` and only
`shop.Plain`, with exit code 0 and no warning. The wrapped header drops `Foo`
the same way. Deleting the call from the base list brings the model back,
so the call is the trigger (see Reproduction Process).

**Affected Components**

- `cli/orm_lens/parser.py`, the Python parser behind the CLI and MCP server:
  - `CLASS_RE` (line 65) captures the bases with `\(([^)]*)\)`, "everything
    up to the first `)`". In the repro that `)` closes `("comment"`, so the
    regex fails to match.
  - `_read_multiline_class()` (line 115) is the fallback for headers
    `CLASS_RE` can't read. It balances the parentheses and joins the lines,
    then retries the same `CLASS_RE` (line 145), so it fails for the same
    reason. `_collect_defs()` calls it only for classes at column 0
    (lines 867-868); a class inside a module-level `if` is out of scope here,
    as the issue says.
  - After a match, the bases are split on `,` with empty parts dropped
    (line 882), and `_looks_like_model()` (line 657) decides model-ness from
    what is left. That is why stripping the call is enough.
- `src/parser.ts`, the TypeScript parser for the VS Code extension, has the
  same `[^)]*` in its `CLASS_RE` (lines 102-103), so the extension likely
  drops these models too. Whether my PR fixes both is the open question in my
  claim, under the repo's parity rule.
- Tests: a new `cli/tests/test_call_in_bases.py`, in the unittest style of
  the existing parser tests, which call `parse_models_file()` on an inline
  source string.

**Toolkit**

- Tool: the issue itself, read end to end, plus one scoped read of
  `cli/orm_lens/parser.py` and `cli/tests/` in my fork, done while writing
  `CLAUDE.local.md` (the unit-6 repo memory file); its subsystem section is
  where these components come from
- What it produced: the call path from `CLASS_RE` through
  `_read_multiline_class()` to `_collect_defs()` and `_looks_like_model()`,
  with line numbers checked against `ac40f8d`, the TS twin in `src/parser.ts`,
  and the test pattern to copy
- Difficulties: none met in the Python side; the issue already named the
  cause and the lines, and they all held. I have not read the TS parser past
  its `CLASS_RE` or run its tests, so the TS impact is still a guess
- What changed: nothing yet. If the maintainer says to fix both parsers, I
  will add the TS call path and `npm test` to `CLAUDE.local.md` first

---

## Reproduction Process

**Environment Setup**

- Windows 11 Pro, Git Bash; Python 3.14.7 in a venv (`python -m venv .venv`,
  activated with `source .venv/Scripts/activate`).
- orm-lens 1.18.2 from my fork (AnthomasGit/orm-lens), on `main` at `ac40f8d`,
  the same commit as upstream `main` on 2026-11-22.
- Setup: `gh repo fork FROWNINGdev/orm-lens --clone`, then
  `cd cli && pip install -e ".[dev]"`. Before touching anything I ran the
  repo's own checks: `python -m pytest -q` gave 1310 passed, 6 skipped, and
  ruff and mypy were clean.
- Django and django-prometheus are not installed. orm-lens only reads source
  files, so the repro needs neither.
- Challenges: none in the project itself. On my side, `gh repo fork` rejected
  `--clone=true` (it takes a bare `--clone`), and I kept `.venv/` out of git
  by adding it to `.git/info/exclude` next to `CLAUDE.local.md`.

**Steps to Reproduce**

1. In an empty folder, create `shop/models.py` with the issue's repro: a
   `Comment(ExportModelOperationsMixin("comment"), models.Model)` class and a
   `Plain(models.Model)` class, each with one `text = models.TextField()`.
2. From that folder, run `orm-lens list -v; echo "exit $?"`.
3. Observed result: `found 1 apps / 1 models`, only `shop.Plain`, exit 0.
   Expected `found 1 apps / 2 models` with `shop.Comment` too. `Comment` is
   dropped with no message.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:**

  ```text
  $ orm-lens list -v; echo "exit $?"
  scanned 1 files in 14ms, found 1 apps / 1 models
  shop.Plain
  exit 0
  ```

  Control, the same file with `ExportModelOperationsMixin("comment"), `
  deleted:

  ```text
  $ orm-lens list -v
  scanned 1 files in 11ms, found 1 apps / 2 models
  shop.Comment
  shop.Plain
  ```

- **My findings:** the bug is real on today's `main`. The call in the base
  list is the trigger: removing it brings `Comment` back. The wrapped header
  from the issue's "How to test" fails the same way (`class Foo(` /
  `ExportModelOperationsMixin("foo"),` / `models.Model,` / `):` gives only
  `shop.Plain`), and deleting the mixin line brings `shop.Foo` back. That
  matches the issue's cause: `CLASS_RE` in `cli/orm_lens/parser.py` captures
  the bases with `[^)]*`, and the multi-line fallback retries the same regex.
  Per the course step, the report stays off the issue; the PR description
  carries the proof.

**Toolkit**

- Tool: repro-check, run twice on this issue: on the claim draft alone before
  posting, then on the claim and the repro report together
  (`repro-check: grade my claim comment in claim-draft.md and my repro report
  in repro-draft.md for issue <url>`)
- What it produced: accept both times. On the pair, all six required checks
  passed (environment, faithful steps, behavior shown, claims backed,
  specific claim, AI disclosure); the preferred template check failed because
  the bug form asks for a VS Code version, which does not apply to a CLI repro
- Difficulties: it flagged three gaps that did not gate the verdict: "the exit
  code is 0" had no output behind it, the wrapped-header case had no control
  and an unclear file, and my last sentence restated the maintainer's own
  analysis. Its verdict also says the report is "ready to post", which the
  course step overrides: the report stays off the issue
- What changed: I re-ran with `echo "exit $?"`, rebuilt the wrapped case with
  the full file plus a control run, and cut the restated cause. I left the VS
  Code field out, since the issue itself omits it

---

## Solution Approach

**Analysis**

The root cause is one regex. `CLASS_RE` in `cli/orm_lens/parser.py`
(line 65) reads the base list as `[^)]*`, so the first `)` ends it. In
`class Comment(ExportModelOperationsMixin("comment"), models.Model):` that
`)` closes `("comment"`, the regex then needs a `:` and finds
`, models.Model):`, and the match fails. The one-line path gives up, and
`_collect_defs()` (lines 867-868) hands the header to
`_read_multiline_class()` (line 115). That fallback balances the parentheses
correctly but then retries the same `CLASS_RE` on the joined buffer
(line 145), so it fails for the same reason and returns no match. The class is
skipped and nothing is logged. Everything after a successful match already
works: the bases are split on `,` with empty parts dropped (line 882), and
`_looks_like_model()` (line 657) decides from the bases that remain, so
`models.Model` alone is enough to make `Comment` a model.

**Proposed Solution**

Follow the maintainer's suggested fix in the issue. In
`_read_multiline_class()`, once the parentheses balance, split the buffer at
the first `(` and strip every call from the base list with
`_CALL_IN_BASES_RE = re.compile(r"[A-Za-z_][\w.]*\s*\([^()]*\)")`, repeated
until nothing changes so nested calls like `Mixin(get_label("x"))` go from the
inside out. Then match `CLASS_RE` on the stripped header. A call's result
can't be resolved statically anyway, and the other bases still decide whether
this is a model.

What I am deliberately not changing:

- `CLASS_RE` itself and the one-line path. A failed one-line match on a
  column-0 `class X(` already falls through to `_read_multiline_class()`, so
  one change there covers both the one-line and the wrapped header.
- Classes indented inside a module-level `if`. The fallback only runs at
  column 0, and the issue puts that case out of scope.
- `Food` and `Keyword` on Tandoor, which are treebeard trees (#330), a
  separate issue.
- `src/parser.ts`, until the maintainer answers my claim's question. If the
  answer is "both", the TS fix goes in the same PR under the parity rule.
- No version bumps; the release checklist is maintainer only.

One side effect to state in the PR: the stripped call no longer appears in
the model's `baseClasses` in the JSON output, so `Comment` lists only
`models.Model`. The issue's fix implies this, and a call has no class name to
report anyway, but a downstream consumer could notice it.

**Implementation Plan**

1. On a branch in my fork (`fix/call-in-bases-335`), add `_CALL_IN_BASES_RE`
   next to `CLASS_RE` in `cli/orm_lens/parser.py`, and strip calls from the
   base list in `_read_multiline_class()` before the `CLASS_RE` match, with a
   short comment naming the ``Mixin("label")`` case.
2. Add `cli/tests/test_call_in_bases.py` (unittest, `parse_models_file()` on
   inline source) for the three cases in the issue's "How to test": the repro
   gives `Comment` and `Plain`, and `Comment` has its `text` field;
   `class Plain(SomeFactory("x")):` is still not a model; the wrapped header
   works. Run it before the fix to watch it fail.
3. Add a `### Fixed` entry under `## [Unreleased]` in `CHANGELOG.md`.
4. Re-run my reproduction (expect `found 1 apps / 2 models`), then
   `python -m pytest -q`, ruff and mypy from `cli/`.
5. If the maintainer asks for both parsers: the same change in
   `src/parser.ts` (`CLASS_RE` at lines 102-103), a test in `test/`, and
   `npm test`.
6. Commit as `fix(parser): keep models with a call in the base list`, and
   open the PR with `Fixes #335` and the PR template filled.

Deviations: none yet.

**Toolkit**

- Tool: none of my course tools did this phase. It came from the issue's own
  suggested fix, my reading of `_read_multiline_class()`,
  `_collect_defs()` and `_looks_like_model()` in my fork, and the
  `CLAUDE.local.md` notes from unit 6. I have not run plan-check on this
  plan yet
- What it produced: the root-cause trace above with line numbers checked
  against `ac40f8d`, a list of what stays out of scope, and a six-step plan
- Difficulties: the TS question is still unanswered on the issue, so step 5
  is conditional; and reading `_looks_like_model()` turned up the
  `baseClasses` side effect, which the issue does not mention
- What changed: I added the `baseClasses` note so the PR states it up front
  instead of a reviewer finding it

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
