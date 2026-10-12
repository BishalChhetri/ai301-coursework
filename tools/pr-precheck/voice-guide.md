### Rule 8 (new this week): a PR description claims only what the diff actually does

A PR description is read by a maintainer deciding whether to merge real code,
not a plan for code not yet written. Every claim must be checkable against
the diff directly.

- **Wrong:** "This cleanly implements the fix with no side effects" (unverifiable, vague).
- **Right:** "Added `filepath.startswith(pattern.lstrip('/'))` to `_should_skip_file`
  in `tech_detector.py`; removed the two `xfail(strict=True)` markers in
  `test_tech_detector.py` that this fix makes resolvable."
