# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

AnthomasGit

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5988945662

Plan for #53, following my reproduction above.

Cause: two parts of the `phone_us` pattern at `safety/pii_scrubber.py:16` each block `(555) 123-4567`. The leading `\b` can't match before `(` after a space, and the only separators allowed are `[-.]?`, so the space in `) 123` breaks the match. The dashed number has neither problem, which is why my step 1 gave `'Call me at (555) 123-4567 or [REDACTED]'` and `[]`.

Two more things I found, with no code changed. `test_us_phone_formats` stops at its first failure, and its fourth format, `+1 555 123 4567`, also passes through today for the same separator reason. The fifth #53 test, `test_mixed_pii_and_text`, has no parenthesized number: with `--runxfail` it fails on `"...TechCorp for [REDACTED]ications..."` because the `street_address` pattern's `Pl` matches inside "applications".

Change, on `fix/53-parenthesized-us-phone` in my fork: replace only `phone_us` with `r"(?<![\w+])(?:\+?1[-. ]?)?(?:\(\d{3}\)|\d{3})[-. ]?\d{3}[-. ]?\d{4}\b"`, remove the xfail marker from the four tests the issue names, and add a test that `detect()` returns `(555) 123-4567` with the `(` inside the span. I won't touch `street_address`, the other patterns, or the marker on `test_mixed_pii_and_text`.

Check: re-running my repro steps, step 1 should print `'Call me at [REDACTED] or [REDACTED]'` and a `phone_us` match for `(555) 123-4567` at 11 to 25, step 2 should give `25 passed, 1 xfailed`, and step 3 `4 passed, 22 deselected`. A monkeypatched scratch run of the new pattern already gave `1 failed, 24 passed` under `--runxfail`, the failure being `test_mixed_pii_and_text`.

Trade-off: allowing spaces means spaced `3 3 4` digit groups like `room 555 123 4567` get redacted. `test_us_phone_formats` requires spaced numbers, and bare `1234567890` is already redacted today.

Should the `test_mixed_pii_and_text` fix go in this PR or a separate issue? A trailing `\b` on `street_address` makes all 25 tests pass in my scratch run. Until I hear back I'll leave its marker and keep this PR to the phone pattern.

---

## Your branch

**Branch**

fix/53-parenthesized-us-phone

**Evidence**

Same environment for both: Windows 11, Git Bash, Python 3.14.7 in `.venv` (`pip install -e ".[dev]"`), pytest 9.1.1. The branch starts at `2f4e82f` so the commit that added `repro_53.py` stays out of the PR. On the branch I ran an identical copy of `repro_53.py` kept outside the repo, which still imports the real `safety.pii_scrubber`.

Before: my unit 2 reproduction as posted (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5903244009), at `2f4e82f`:

```
$ .venv/Scripts/python repro_53.py
'Call me at (555) 123-4567 or [REDACTED]'
2026-09-29 21:42:56 [info     ] pii_detected                   count=0 types=0
[]
2026-09-29 21:42:56 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]

$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py -q -rxX -p no:cacheprovider
..xx.......x.....x....x..                                                [100%]
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
20 passed, 5 xfailed in 3.15s

$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py --runxfail -q --tb=line -p no:cacheprovider -k "test_us_phone_number_redaction or test_us_phone_formats or test_detect_phone_pii or test_phone_at_start_of_text"
FFFF                                                                     [100%]
E   AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
E   AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
E   assert 0 > 0
     +  where 0 = len([])
E   AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii
FAILED tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text
4 failed, 21 deselected in 1.51s
```

After: the same three steps on `fix/53-parenthesized-us-phone` at the fix commit `a10ab3f`:

```
$ git rev-parse --short HEAD
a10ab3f
$ .venv/Scripts/python repro_53.py
'Call me at [REDACTED] or [REDACTED]'
2026-10-05 01:22:53 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
2026-10-05 01:22:53 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py -q -rxX -p no:cacheprovider
.......................x..                                               [100%]
=========================== short test summary info ===========================
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
25 passed, 1 xfailed in 2.96s
$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py --runxfail -q --tb=line -p no:cacheprovider -k "test_us_phone_number_redaction or test_us_phone_formats or test_detect_phone_pii or test_phone_at_start_of_text"
....                                                                     [100%]
4 passed, 22 deselected in 2.40s
```

These match what the plan said to expect. The parenthesized number is now redacted and detected with the `(` inside the span, and the dashed control is unchanged. The four named tests pass without their markers, and only `test_mixed_pii_and_text` is still xfailed (it's the `street_address` bug, out of scope). The total goes from 25 to 26 tests because of the new `test_detect_parenthesized_phone_span`, which is why step 3 now shows 22 deselected.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. 15/20 (first full run; below the 18/20 bar)
2. 8/10 (`--only` on the five misses plus five canaries, after my first round of rubric edits; partial, not scored)
3. 6/8 (`--only` on pkg-09 and pkg-14 plus six canaries, after my second round of edits; partial, not scored)
4. 20/20 (full run saved with `--save-run` as `eval-run.txt`; bar 18/20: PASS)

I didn't edit anything between runs 3 and 4. The file fingerprints in `eval-run.txt` (`rubric.md sha256:87cc9948f28ef2c5`, `procedure.md sha256:929b5e5d541002c1`) match the files as I left them after the second round. So the gap between 6/8 and 20/20 is the Sonnet grader varying from run to run on the same rubric, not a fix.

**Package analysis**

pkg-14 (zellij-org/zellij#5174): my rubric decided accept, and the gold label is accept. On my first full run it was the worst miss, failing `diagnosis, scope, approach, test, unknowns`. Its gold note reads "defers the untestable Windows variant and says so; arguable on the deferral, ready as scoped."

My final rubric reads it as accept for these reasons:

- **scope:** the plan says "Explicitly deferred, with reasons: the Windows session-switch variant ... (I cannot test Windows ...) and any change to how theme detection caches its results." The cache only correlates with the leak ("the next attach is clean, the one after leaks again"), and the repro doesn't place the cause there. My scope check now passes a deferral like that when "the plan states the deferral and a reason, and the planned change still fixes the reproduced failing step."
- **scope and approach:** "exact functions to be pinned in the PR after tracing the query issuance with debug logs" passes now, because the scope row allows leaving files or functions open "if the plan says so and says how it will pin them."
- **test:** "5 consecutive SSH reattach cycles with no rgb strings in any pane" re-runs the repro's failing step with a stated outcome. That counts even though it isn't an automated test.
- **diagnosis and unknowns:** the reattach-handshake explanation sits inside the component the repro pins down (fresh attach clean, reattach leaks, 0.44.1 clean). "0.44.1, which predates the reattach-path change" is version history, which my unknowns check now lists as not an unknown.

**Check rationale**

From `tools/plan-check/rubric.md`, exactly as it reads now:

| unknowns | The plan's load-bearing claims (the stated cause, and that the planned change removes the reproduced symptom), read against what the repro evidence and repo facts establish | Fails if the plan states as fact a load-bearing claim that the evidence contradicts, or a cause the evidence doesn't localize at all, without marking it as an assumption or open question. Otherwise passes. A code-level explanation that is consistent with the pinned facts and sits inside the component they localize is not an unknown. Neither are implementation details (line numbers, function names, counts of call sites, version history) or who in the thread suggested what. | required |

It took two revisions to get here. My first version made the plan mark every claim the evidence didn't establish. That rejected three clear accepts (pkg-02, pkg-05, pkg-09) over small details every real plan has. In the second version I limited it to load-bearing claims: "Passes if each load-bearing claim is either established by the repro evidence or repo facts, or marked as an assumption or open question." That fixed pkg-02 and pkg-05, but pkg-09 still failed on its code-level explanation ("the normalization globset would do in `Candidate::new` is skipped when the extracted regex is used directly") and pkg-14 failed on version history. No repro can pin an internal code path. So I flipped the check to fail only on claims the evidence contradicts or causes it doesn't localize at all, and named the things that don't count as unknowns.

**Trade-offs**

Loosening `unknowns` and `diagnosis` risked flipping the wrong-cause packages to accept, since those are exactly the plans that state a cause with confidence. I put the four wrong-cause packages (pkg-01, pkg-07, pkg-11, pkg-16) in the `--only` run as canaries, and the confirming full run kept `wrong-cause 4/4`. They still fail because their causes contradict the repro, which fails both checks.

The case I accept this will miss: a plan whose mechanism is wrong but still sits inside the component the repro localizes, with no repro step that contradicts it. It now passes `diagnosis` and `unknowns`, because the rubric treats any code-level explanation consistent with the pinned facts as supported.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
