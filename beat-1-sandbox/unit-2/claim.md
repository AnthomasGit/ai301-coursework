Hi, I'd like to take this one as my first PathReview contribution.

The bug as reported: `PIIScrubber.scrub()` returns `(555) 123-4567` untouched and `detect()` gives `[]` for it, while the dashed `555-123-4567` in the same string becomes `[REDACTED]`.

Plan before touching any code: set up from `docs/SETUP.md` on Windows 11 against current `main`, run the issue's snippet with the dashed number as a control, then run the tests in `tests/unit/test_pii_scrubber.py` marked `xfail` for #53 with `--runxfail` so the actual assertion failures show. I'll post my environment, the exact commands, and their output here as a reproduction report. After that I'll look at the `phone_us` pattern in `safety/pii_scrubber.py`.
