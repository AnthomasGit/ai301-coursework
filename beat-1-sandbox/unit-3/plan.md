# Plan for #53: PII scrubber misses parenthesized US phone numbers

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/53#issuecomment-5903244009
Branch (on my fork, AnthomasGit): `fix/53-parenthesized-us-phone`

## Repro evidence

From my repro comment (commit `2f4e82f`, Windows 11, Python 3.14.7, pytest 9.1.1):

Step 1, the issue's snippet with the dashed number as a control:

```
$ .venv/Scripts/python repro_53.py
'Call me at (555) 123-4567 or [REDACTED]'
2026-09-29 21:42:56 [info     ] pii_detected                   count=0 types=0
[]
2026-09-29 21:42:56 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

Step 2, the shipped tests: "Five tests carry the #53 reason, one more than the four the issue names"

```
20 passed, 5 xfailed in 3.15s
```

Step 3, the four named tests with `--runxfail`:

```
E   AssertionError: assert '[REDACTED]' in 'Call me at (555) 123-4567'
E   AssertionError: assert '[REDACTED]' in 'Contact: (555) 123-4567'
E   assert 0 > 0
     +  where 0 = len([])
E   AssertionError: assert '[REDACTED]' in '(555) 123-4567 is my phone number.'
4 failed, 21 deselected in 1.51s
```

In the same string, the dashed number is redacted and detected and the parenthesized one isn't, wherever it sits in the text. `scrub()` and `detect()` both loop over `PII_PATTERNS`, so the cause is the `phone_us` pattern.

Two more facts I found while planning, on `70e74ee` (that's `2f4e82f` plus the committed `repro_53.py`, no code change):

1. The fifth #53 test, `test_mixed_pii_and_text`, has no parenthesized number. With `--runxfail` it fails because the `street_address` pattern's `Pl` alternative matches inside "applications":

   ```
   $ .venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py --runxfail -q --tb=short -p no:cacheprovider -k "phone or mixed"
   E   assert 'Python' in "\n        Professional Background:\n        I worked at TechCorp for [REDACTED]ications.\n ...
   5 failed, 2 passed, 18 deselected in 5.98s
   ```

2. `test_us_phone_formats` stops at its first failing format. The fourth format, `+1 555 123 4567`, also passes through today:

   ```
   'Contact: +1 555 123 4567' -> 'Contact: +1 555 123 4567'
   ```

## Diagnosis

```python
"phone_us": r"\b(?:\+?1[-.]?)?\(?([0-9]{3})\)?[-.]?([0-9]{3})[-.]?([0-9]{4})\b",
```

Two parts of this pattern each block `(555) 123-4567`. The leading `\b` can't match before `(` when a space or the start of the string precedes it, so no match starts at the parenthesis. And the separators are `[-.]?` only, so a match starting at `555` breaks on the space in `) 123`. The dashed control has neither problem. The separator rule also explains `+1 555 123 4567`.

I checked this by monkeypatching the new pattern (below) in a scratch run without editing the repo. Step 1 printed `'Call me at [REDACTED] or [REDACTED]'`, and the full test file under `--runxfail` gave `1 failed, 24 passed`. The one failure was `test_mixed_pii_and_text`.

## Scope

Files I'll touch:

- `safety/pii_scrubber.py`: the `phone_us` entry only.
- `tests/unit/test_pii_scrubber.py`: drop the xfail marker from the four named tests (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`) and add one span test.

Out of scope:

- `street_address` and the marker on `test_mixed_pii_and_text`. That failure is a separate bug (fact 1), and removing the marker would make the test fail. I'll ask on the thread whether to include it.
- The other patterns, the `scrub()`/`detect()` loops (including the `# noqa: B007` line), and the `E501` suppression in `pyproject.toml`.
- `repro_53.py`. It's on my fork's `main`, so I'll branch from `2f4e82f` and run a copy from outside the repo.

## Approach

1. Copy `repro_53.py` out of the repo, then `git switch -c fix/53-parenthesized-us-phone 2f4e82f`. Run steps 1 to 3 and save the "before" output.
2. Replace `phone_us` with:

   ```python
   "phone_us": r"(?<![\w+])(?:\+?1[-. ]?)?(?:\(\d{3}\)|\d{3})[-. ]?\d{3}[-. ]?\d{4}\b",
   ```

   The area code is either fully parenthesized or bare. A literal space joins `-` and `.` as a separator (`\s` isn't allowed, so line breaks don't count). The lookbehind replaces `\b` so a match can start at `(` or `+` but not mid-word. Nothing reads the old capture groups, and no code outside the module and its tests uses `PIIScrubber`.
3. Remove the four xfail markers.
4. Add `test_detect_parenthesized_phone_span`: `detect("Call me at (555) 123-4567")` returns one `phone_us` entry with value `(555) 123-4567`, and `text[start:end]` equals it. A half-fix that only loosens separators would match `555) 123-4567` and fail this test.
5. Run `make lint`, `make typecheck` and `make test-unit`, then commit `fix(safety): redact parenthesized and space-separated US phone numbers` with `Fixes #53`.

## Test plan

I'll re-run my repro steps on the branch.

Step 1 (the `repro_53.py` copy). Expected:

```
'Call me at [REDACTED] or [REDACTED]'
... pii_detected count=1 types=1
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
... pii_detected count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]
```

Step 2 (`.venv/Scripts/python -m pytest tests/unit/test_pii_scrubber.py -q -rxX -p no:cacheprovider`). Expected: `25 passed, 1 xfailed`, with `test_mixed_pii_and_text` the only XFAIL and no `XPASS(strict)`.

Step 3 (same `--runxfail -k` command). Expected: `4 passed, 22 deselected`. It's 22 now because of the new test.

On current code, step 1 shows the unchanged number and `[]`, step 3 shows `4 failed`, and the new span test fails. `make test-unit`, `make lint` and `make typecheck` should stay clean.

## Risks and unknowns

- With spaces allowed, any spaced `3 3 4` digit group gets redacted, for example `room 555 123 4567` in my scratch run. `test_us_phone_formats` requires spaced numbers to be redacted, and bare ten-digit runs like `1234567890` are already redacted. A maintainer may want it narrower.
- Unknown: how maintainers want the fifth test handled. If they want it in this PR, adding a trailing `\b` to `street_address` made all 25 tests pass in scratch, and I'll log that under Deviations.
- Assumption: nothing depends on the old capture groups or on `+1-...` keeping its `+`. A repo search outside `.venv` found `PIIScrubber` only in the module, its tests, and docs. CI will confirm.
- `detect()` now types `+1 555 123 4567` as `phone_us`. Today it isn't detected at all, so no test depends on its type.
- I only ran on Windows. The change is a plain regex, and CI runs on Linux.

## Deviations

None. The plan held. Commit `a10ab3f` on `fix/53-parenthesized-us-phone` changes only the `phone_us` line, removes the four markers, and adds the span test, and every check came out as predicted:

```
'Call me at [REDACTED] or [REDACTED]'
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]
25 passed, 1 xfailed in 2.77s
4 passed, 22 deselected in 2.58s
```

The new span test failed on the old regex (`assert 0 == 1`) and passes on the fix. ruff, black and mypy are clean, and `tests/unit` gives `380 passed, 49 xfailed`. That's four fewer xfails than before, one per marker removed.

Two small things during the build, neither a change to the plan. A `fix/53-parenthesized-us-phone` branch already existed, created from `main` at `70e74ee`, so I reset it to `2f4e82f` as the plan says, to keep `repro_53.py` out of the PR. And `make` isn't installed in my Git Bash, so I ran the commands behind `make lint`, `make typecheck` and `make test-unit` directly from `.venv`. CI will run the real targets.

The `test_mixed_pii_and_text` question is still open on the thread, and its marker is still in place.
