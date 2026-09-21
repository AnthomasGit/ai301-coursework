# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
paste the output here, including the closing JSON block
```

❯ issue-select: grade these candidate first issues: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/37 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53

I'll load the issue-select skill first.

● Skill(issue-select)
Successfully loaded skill

I'll read the skill's rubric, scope, and evidence guide first.

  Ran 10 shell commands

All three candidates are in the scoped repo (codepath/pathreview-ai301-fa26-s3), so all three are eligible. e against today, 2026-09-20.
                                                                                                            Shared repo facts: pushed_at: 2026-09-16T2ed: false, 71 open issues.
                                                                                                            Verdict: all three accepted — every requirboth preferred checks pass on each too.
                                                                                                            ────────────────────────┬──────────────────────────────┐
│          Check           │        #36        │        #37        │        #53        │                    ────────────────────────┼──────────────────────────────┤
│ Active Repo (push ≤90d)  │ pass — 4d         │ pass — 4d         │ pass — 4d         │                    ────────────────────────┼──────────────────────────────┤
│ Issue Open Date (<90d)   │ pass — 10d        │ pass — 10d        │ pass — 10d        │                    ────────────────────────┼──────────────────────────────┤
│ Assigned (none)          │ pass              │ pass              │ pass              │                    ────────────────────────┼──────────────────────────────┤
│ Comments Checked (pref.) │ pass — 0 comments │ pass — 0 comments │ pass — 0 comments │                    ────────────────────────┼──────────────────────────────┤
│ Has Fix (no Fix section) │ pass              │ pass              │ pass              │                    ────────────────────────┼──────────────────────────────┤
│ Good First (pref.)       │ pass              │ pass              │ pass              │                    ────────────────────────┴──────────────────────────────┘
                                                                                                            Timelines confirm no linked PRs, no claim nts on any of the three — they are unclaimed even before the Path Review house rule (classmates' claims don't block) is applied.

Two things worth your attention:                                                                            
1. I can't rank them. scope.md's fit profile is still the placeholder (Write a few sentences here.). With nolanguages, goals, or avoid-list, the tir has no input — and the three tie on everyrubric check, preferred ones included. The JSON is in issue-number order, which is not a ranking. Fill infit profile and I'll re-rank.
2. Rubric gap — Family 3 (newcomer scope) is barely covered. Has Fix is the only check touching the issue's content, and it only asks that a "Fix" ades how big the work is, so an umbrella ortracking issue would sail through this rubric unchanged. As it happens these three are all genuinely small (#36 and #37 both state "Estimated effort: 2gex in pii_scrubber.py with a runnable repro and four named failing tests), so the gap didn't bite here — but it will on a worse candidate. Also uncovered: the fifth surface, the repo's AI-contrirse workflow makes relevant.
[
   {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53",
    "checks": [
      {"name": "Active Repo", "grade": "pa_at 2026-09-16T21:50:20Z, 4 days before2026-09-20"},
      {"name": "Issue Open Date", "grade":_at 2026-09-10T21:40:38Z, 10 days before2026-09-20"},                                                                          {"name": "Assigned", "grade": "pass"and no assigned event in the timeline"},
      {"name": "Comments Checked", "grade": "pass", "evidence": "comments: 0, so no maintainer comment saissue is being closed"},
      {"name": "Has Fix", "grade": "pass", "evidence": "body has only a 'Steps to reproduce' subsection prelated-tests line; no Fix subsection"},
      {"name": "Good First", "grade": "pass", "evidence": "labels: ['bug', 'good first issue', 'safety', 'tier-1']"}
    ],
    "verdict": "accept"
  }
]
---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

run 1:agreement: 16/20
run 2:agreement: 18/20
run 3:agreement: 13/20
run 4:agreement: 18/20
run 4:agreement: 19/20
**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

'issue-14'
Rubric's decision: both accepted and rejected on different trials with no changed rubric
Gold label: accepted
Reasoning: The check finds the absence of a 'fix' subsection and returns pass. Issue 14 does not have a fix subsection but there are comments referencing the fix to the issue. The LLM in some runs takes those comments and understands them as the 'fix' subsection. Thus returns fail. In other runs it does not acknowledge the comments as a fix subsection (which it is not) and returns pass.

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

"| Issue Open Date | Issue section, the "opened by <user> on <date>" line | opened less than 90 days before the snapshot date or labels include "stale::recovered" | required |"

Rationale: Checks the open date of the issue. I don't want to work on a stale or abandoned issue from previous releases. Unless it has been revived. No exceptions.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

I accept it will miss older issues that are not explicitly labeled as recovered.
If there is an issue opened later than 90 days that is not stated as recovered the check will fail. In this case even if the issue is active, demonstrated by owner comments. The check will fail and reject the issue. Thus missing needed resolved issues.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
   It fits my interest in parsing and data validation, since the fix is a single regex in pii_scrubber.py. I'm new to PIIScrubber, but the four named failing tests define the expected behavior. It's roughly an afternoon of work, which fits my time.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
   The verdict confirmed the repo is active, the issue is recent and unassigned, and no fix exists yet. What the rubric couldn't measure is how ready it is to start: it has copy-paste repro steps and named failing tests, and it's pure backend Python I can handle.
3. The anticipated difficulty in claiming it.
   Solving it should be easy. Claiming might be hard. Clear, small issues get picked up fast, so I'd check for assignees or linked PRs and comment right away to ask for it. ]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
