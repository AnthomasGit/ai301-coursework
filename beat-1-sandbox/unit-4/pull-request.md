# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/251

**Branch**

`fix/53-parenthesized-us-phone`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 0/0 scored items`. All 20 packages errored before grading. On Windows the harness pipes the prompt to `claude` in the cp1252 codepage, which can't encode the arrow characters I had in `procedure.md` and the evidence guide, so `claude` got empty stdin. I replaced the arrows with words and re-ran with `PYTHONUTF8=1`.
2. Full run: `agreement: 18/20 scored items  (bar: 18/20: PASS)`. pkg-05 and pkg-11 disagreed (both gold `accept`, both my tool `reject`).
3. Partial run, `--only pkg-05,pkg-11,pkg-04,pkg-14,pkg-17,calib-03,calib-04 --include-calibration`: `agreement: 4/5 scored items`. pkg-11 now agreed and every canary held; pkg-05 still disagreed.
4. Partial run, `--only pkg-05`: 1/1. pkg-05 agreed, with `repro-evidence` graded `pass`.
5. Full run, saved with `--save-run eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

pkg-11 (BurntSushi/ripgrep#3477). My rubric decided `reject` on run 2, with the table note `failed: claims-match-diff, checks-shown`. The gold label is `accept`: "terse but complete: escape-aware trailing-space trim with git-parity evidence, tests for all three escape cases, own-words AI disclosure per the repo's stated policy".

The description says "One function touched, tests added for the escaped, unescaped, and double-backslash cases." The diff adds one test function, `fn escaped_trailing_space_is_kept()`, and covers the three cases as assertions inside it, including `assert!(gi.matched("foo ", false).is_ignore());` and `assert!(gi.matched("foo\\", false).is_ignore());`. My `claims-match-diff` check said at the time "A claim of tests for N named cases needs a test or fixture for each case.", so the grader counted one test for three named cases and failed the claim. That condition came from my calib-03 worksheet, where the checklist promised tests for the all-comments control and nested chains and the diff only added `blank-line-before-else.js`. I wrote it to catch missing cases, but its wording counted test functions instead of cases, so it also failed a PR whose tests were all there.

**Check rationale**

From `tools/pr-precheck/rubric.md`, the `claims-match-diff` row's pass condition:

> Passes if a diff line bears out every concrete claim. A claim that something was not added passes when no hunk adds it. A claim of tests for N named cases needs each case's input to be exercised somewhere in the added tests or fixtures. A case covered by an assertion inside a shared test counts; one test function can cover several cases. A fidelity claim ("exactly as planned", "no changes beyond the plan") fails whenever the diff holds a file or hunk that plan-fidelity's conditions (1) or (2) would fail. The check fails in either direction: the description claims something the diff does not deliver, or the diff carries something the description says it does not.

The test sentence used to read "A claim of tests for N named cases needs a test or fixture for each case." That rejected pkg-11, whose three cases sit as assertions in one test function. I rewrote it to count cases, not functions: each named case's input has to be exercised somewhere in the added tests. I kept the claim-by-claim rule itself because calib-03 is the reason the check exists. Its description says "Implements the plan on #19727 exactly, with no changes beyond it" over a diff that rewrites `src/language-js/print/property.js`, and its checklist promises "format fixtures for the issue's input, the all-comments control, and nested chains" when the diff adds one fixture. The rewrite still fails calib-03, because the control and nested-chain inputs appear in no added test.

**Trade-offs**

Loosening `claims-match-diff` (and, in the same revision, `repro-evidence`, so a fix item other than the repro's failing step can be shown by one sentence naming the observed result) could have flipped rejects to accepts. So I re-ran canaries with `--only` before the confirming full run: `pkg-04` and `pkg-14` (not-tested), `pkg-17` (silent-drift, the description claims docs the diff doesn't contain), and calibration packages `calib-03` (the description-versus-diff trap) and `calib-04` (a second failure mode the evidence never re-runs). All five still came out `reject`, matching gold. The full run that followed agreed on all 20, so nothing changed elsewhere.

The case I accept it will miss: a description that names a test case whose input appears in an added test only by coincidence, for example the same string used in an unrelated assertion. The check now looks for the case's input, not for a test written for it, so it can pass a claim like that.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
