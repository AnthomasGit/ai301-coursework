Reproduction report for #53.

Result: reproduced. `(555) 123-4567` is not redacted by `scrub()` and not found by `detect()`, while `555-123-4567` is handled by both. This matches the issue.

**Environment**

- Code: a fresh clone of this repo at commit `2f4e82f` (current `main`), working tree clean
- OS: Windows 11 (10.0.26200), 64-bit, commands run in Git Bash
- Python: 3.14.7 in a `.venv` created with `python -m venv .venv` and `pip install -e ".[dev]"` (pytest 9.1.1)
- Setup: I skipped Docker and `make setup` from `docs/SETUP.md`. This module and its unit tests don't use the database.

**Step 1: the issue's snippet**, saved as `repro_53.py` in the repo root, plus one extra line that runs the dashed number as a control:

```python
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(repr(s.scrub('Call me at (555) 123-4567 or 555-123-4567')))
print(s.detect('Call me at (555) 123-4567'))
print(s.detect('Call me at 555-123-4567'))
```

```
$ .venv/Scripts/python repro_53.py
'Call me at (555) 123-4567 or [REDACTED]'
2026-09-29 21:42:56 [info     ] pii_detected                   count=0 types=0
[]
2026-09-29 21:42:56 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

**Step 2: the test file as it ships.** The tests for this issue are marked `xfail(strict=True)`, so a normal run reports them as xfailed. Five tests carry the #53 reason, one more than the four the issue names:

```
$ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py -q -rxX -p no:cacheprovider
..xx.......x.....x....x..                                                [100%]
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
20 passed, 5 xfailed in 3.15s
```

**Step 3: the four named tests with the marker ignored**, so the real assertions show. I kept only the `E` lines and the summary:

```
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

Expected: both phone formats are replaced with `[REDACTED]`, and `detect()` returns a `phone_us` entry for the parenthesized one.

Actual: the parenthesized number comes back unchanged and `detect()` returns `[]` for it (step 1). All four named tests fail on that same input when the marker is ignored (step 3). The dashed number is redacted and detected, which is the control.

I haven't changed any code. Next I'll read the `phone_us` pattern in `safety/pii_scrubber.py` and post what I find before proposing a fix.
