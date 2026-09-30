# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream
AnthomasGit

---

## Posted upstream

**Claim comment**
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5903137013

Hi, I'd like to take this one as my first PathReview contribution.

The bug as reported: PIIScrubber.scrub() returns (555) 123-4567 untouched and detect() gives [] for it, while the dashed 555-123-4567 in the same string becomes [REDACTED].

Plan before touching any code: set up from docs/SETUP.md on Windows 11 against current main, run the issue's snippet with the dashed number as a control, then run the tests in tests/unit/test_pii_scrubber.py marked xfail for #53 with --runxfail so the actual assertion failures show. I'll post my environment, the exact commands, and their output here as a reproduction report. After that I'll look at the phone_us pattern in safety/pii_scrubber.py.

**Reproduction comment**
https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5903244009

Reproduction report for #53.

Result: reproduced. (555) 123-4567 is not redacted by scrub() and not found by detect(), while 555-123-4567 is handled by both. This matches the issue.

Environment

Code: a fresh clone of this repo at commit 2f4e82f (current main), working tree clean
OS: Windows 11 (10.0.26200), 64-bit, commands run in Git Bash
Python: 3.14.7 in a .venv created with python -m venv .venv and pip install -e ".[dev]" (pytest 9.1.1)
Setup: I skipped Docker and make setup from docs/SETUP.md. This module and its unit tests don't use the database.
Step 1: the issue's snippet, saved as repro_53.py in the repo root, plus one extra line that runs the dashed number as a control:

from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(repr(s.scrub('Call me at (555) 123-4567 or 555-123-4567')))
print(s.detect('Call me at (555) 123-4567'))
print(s.detect('Call me at 555-123-4567'))
$ .venv/Scripts/python repro_53.py
'Call me at (555) 123-4567 or [REDACTED]'
2026-09-29 21:42:56 [info     ] pii_detected                   count=0 types=0
[]
2026-09-29 21:42:56 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
Step 2: the test file as it ships. The tests for this issue are marked xfail(strict=True), so a normal run reports them as xfailed. Five tests carry the #53 reason, one more than the four the issue names:

$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py -q -rxX -p no:cacheprovider
..xx.......x.....x....x..                                                [100%]
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
20 passed, 5 xfailed in 3.15s
Step 3: the four named tests with the marker ignored, so the real assertions show. I kept only the E lines and the summary:

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
Expected: both phone formats are replaced with [REDACTED], and detect() returns a phone_us entry for the parenthesized one.

Actual: the parenthesized number comes back unchanged and detect() returns [] for it (step 1). All four named tests fail on that same input when the marker is ignored (step 3). The dashed number is redacted and detected, which is the control.

I haven't changed any code. Next I'll read the phone_us pattern in safety/pii_scrubber.py and post what I find before proposing a fix.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. 12/20 (first rubric: exact-match environment, steps, and output checks)
2. 16/20 (revised rubric: six required checks plus one preferred; stated environment and trigger differences allowed)
3. 9/9 partial `--only` run (4 disagreements + 5 canaries; not scored)
4. 19/20 (final full run, saved as `eval-run.txt`; bar 18/20 PASS)


**Package analysis**

pkg-03 (BurntSushi/ripgrep#2779): my rubric said reject, gold says accept. It failed "Environment recorded." The issue reports "ripgrep 13.0.0 … Kubuntu 23.10, installed via APT"; the report says "ripgrep 15.2.0 (cargo install), Arch Linux (x86_64). The issue was filed against 13.0.0; behavior is unchanged on 15.2.0." My check requires "every version or platform difference from what the issue states is named outright." The version change is named, but the OS and install-method changes aren't, so the grader counted them as silent deviations. Gold accepts because the proof is sound: the exact file and command from the issue, output that matches the issue's, and a control run without `-r`. My rubric is stricter than it needs to be on differences that don't affect the bug.

**Check rationale**

Check:| AI disclosure when required | **Repo facts** → contribution policy, read against the claim comment and repro report (Disclosure family). | Treat every package as AI-assisted work. If the policy requires disclosing AI use in issue comments or in any contribution "in any form", the comments must disclose the tool and the extent of assistance. Passes automatically when the policy has no disclosure requirement, only requires disclosure in pull requests, or only asks that comments be human-written in the contributor's own words. | required |

My first rubric had no disclosure check, so it couldn't reliably fail pkg-20 (ghostty requires disclosing "all AI usage in any form"). A plain "must disclose" rule would have wrongly rejected pkg-03, pkg-05, and pkg-09, whose repos don't require disclosure in comments. So the check names exactly when disclosure applies and passes everything else.



**Trade-offs**

Loosening "Faithful, followable steps" and "Claims backed" flipped pkg-03, pkg-05, pkg-10, and pkg-12 to accept. To check nothing weak slipped through, I re-ran canaries with `--only`: pkg-08, pkg-13, pkg-15, pkg-17, and pkg-18 all still rejected (9/9). The case I accept it will miss: a vague setup the grader reads as rebuildable when a stranger couldn't actually rebuild it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
