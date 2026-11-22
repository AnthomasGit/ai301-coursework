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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
