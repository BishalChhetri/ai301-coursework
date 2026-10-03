# Plan: fix vendored/build-output files skewing language detection (#57)

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57
Repro (Unit 2): https://github.com/codepath/pathreview-ai301-fa26-s3/issues/57#issuecomment-5862274211
Branch: `fix/57-root-level-skip-patterns`

## Diagnosis

Confirmed by direct testing against `_should_skip_file` (see Unit 2 repro report,
linked above): calling it on all eight paths in the issue's reproduction,
including the six vendored/build files, returns `skipped=False` for every one
of them.

```
'main.py'                        skipped=False
'core/app.py'                    skipped=False
'node_modules/lib/index.js'      skipped=False
'node_modules/lib/util.js'       skipped=False
'node_modules/x/a.js'            skipped=False
'node_modules/y/b.js'            skipped=False
'build/bundle.js'                skipped=False
'build/vendor.js'                skipped=False
```

The root cause is in `_should_skip_file`'s pattern list:

```python
skip_patterns = [
    "/node_modules/",
    "/vendor/",
    "/dist/",
    "/build/",
    "/.git/",
    "/__pycache__/",
    "/.venv/",
    "/venv/",
]
return any(pattern in filepath for pattern in skip_patterns)
```

Every pattern has a **leading slash**. This substring check only matches when the
directory is nested under something else (e.g. `src/node_modules/x.js`), never
when it's a top-level, repo-root-relative path like `node_modules/lib/index.js`
or `build/bundle.js` — which is exactly how the issue's file list (and most real
repo-relative file listings) are formatted. Confirmed directly in Unit 2:

```
"/node_modules/" in "node_modules/lib/index.js"      -> False
"/node_modules/" in "src/node_modules/lib/index.js"  -> True
"/build/" in "build/bundle.js"                        -> False
```

This is why none of the six vendored/build files are skipped today — the bug is
that they fail to match any skip pattern and so are treated as ordinary source
files. Once the fix makes them match correctly, they'll be excluded from
language detection, leaving only `main.py` and `core/app.py` to determine the
primary language. `_detect_tech`'s primary-language selection
(`tech_detector.py:122-123`) is `sorted(languages)[0]` — alphabetically first
among the detected languages, not a count comparison. With the vendored/build
files correctly excluded, `languages` contains only `{"Python"}`, so
`primary_language` becomes `"Python"` regardless of this alphabetical-vs-count
distinction. (Before the fix, `languages` contains `{"JavaScript", "Python"}`,
and `"JavaScript"` sorts before `"Python"`, which is why the bug manifests as
`"JavaScript"` rather than being masked by it.)

## Scope

In scope:

- The pattern-matching logic inside `_should_skip_file` in
  `agent/tools/tech_detector.py`.
- `tests/unit/test_tech_detector.py`: removing the `@pytest.mark.xfail(strict=True,
reason="issue #57: ...")` markers on `test_node_modules_excluded` (line ~66-68)
  and `test_build_directory_excluded` (line ~97-99). Per `docs/CONTRIBUTING.md`,
  once a fix makes a `strict=True` xfail test pass, CI reports `XPASS(strict)` as
  a failure until the marker is deleted — removing these two markers is part of
  closing this issue, not a separate follow-up.

Not in scope: the primary-language selection logic itself (`_detect_tech`'s
`lang_list = sorted(languages); primary = lang_list[0]`, alphabetical-first
rather than most-common). A classmate (hworku24) raised this same point on the
issue thread. This is a separate design question from the issue's reported bug
and from the two named failing tests; changing it would be a larger, unrelated
behavior change. Flagged under Risks below, and acknowledged in the plan comment.

## Files to touch

- `agent/tools/tech_detector.py` — fix `_should_skip_file`'s pattern matching so
  it correctly recognizes top-level, repo-root-relative vendor/build directories,
  not just nested ones.
- `tests/unit/test_tech_detector.py` — remove the two `xfail(strict=True)`
  markers on `test_node_modules_excluded` and `test_build_directory_excluded`,
  per CONTRIBUTING's rule that a seeded bug's strict xfail marker is removed once
  the fix makes the test pass.

## Approach

Replace the plain substring check with one that correctly matches a directory
component at any position in the path, including the start. The simplest robust
fix: normalize each skip pattern to also match when the filepath _starts with_
the un-prefixed directory name (stripping the pattern's leading slash for that
comparison), in addition to the existing nested-path substring check. Concretely,
change:

```python
return any(pattern in filepath for pattern in skip_patterns)
```

to a check that also covers the top-level case, e.g.:

```python
return any(
    pattern in filepath or filepath.startswith(pattern.lstrip("/"))
    for pattern in skip_patterns
)
```

This keeps the existing nested-path behavior (`src/node_modules/...` still
matches via the substring check) while fixing the top-level case
(`node_modules/...` now matches via `startswith`). This is a minimal,
two-file change — well under the rubric's size guidance.

## Test plan

Re-run the exact Unit 2 repro steps against the fixed code:

```python
from agent.tools.tech_detector import TechDetector
t = TechDetector()
files = ['main.py','core/app.py','node_modules/lib/index.js','node_modules/lib/util.js','node_modules/x/a.js','node_modules/y/b.js','build/bundle.js','build/vendor.js']
result = t.execute({'files': files})
print(result.data)
```

Expected after the fix: `primary_language` changes from `'JavaScript'` to
`'Python'`, and `all_languages` contains only `['Python']` (since all six JS
files should now be correctly filtered out).

Also, after removing the two xfail markers, run the repo's own named tests,
which should now pass (not xfail, not error):

```
pytest tests/unit/test_tech_detector.py -k "test_node_modules_excluded or test_build_directory_excluded"
```

As a regression check, re-run the nested-path case to confirm it still works:

```python
files_nested = ['main.py', 'src/node_modules/lib/index.js']
result_nested = t.execute({'files': files_nested}).data
assert result_nested['primary_language'] == 'Python', (
    f"expected 'Python', got {result_nested['primary_language']!r}"
)
```

## Risks and unknowns

- I haven't seen the full test suite for `tech_detector.py`, so there may be
  existing tests that assert specific behavior for edge cases (e.g. a file
  literally named `build` with no trailing content, or a path containing
  `build` as a substring of a different word, like `builder.py`) that my fix
  could interact with. I'll run the full test file, not just the two named
  tests, before considering this done.
- I'll add a regression test case covering a similarly-named top-level
  file/directory that shouldn't be excluded (e.g. `build_config.py`), to
  confirm the fix doesn't over-match names that merely start with a skip
  pattern's text.
- `_detect_tech`'s alphabetical "primary language" selection
  (`sorted(languages)[0]`) is out of scope per the Scope section above — raised
  independently by hworku24 on the thread — but a maintainer may still want to
  revisit it separately, since it means primary language is decided by
  alphabetical order rather than file count whenever this bug isn't in play
  either.
- I have not yet confirmed there are no other tests in the suite referencing
  these two xfail markers by name (e.g. a test asserting the marker's reason
  string) that would need updating alongside their removal.

## Deviations

Nothing changed; the plan held. I made the exact fix described in the plan:
added `filepath.startswith(pattern.lstrip("/"))` to `_should_skip_file`'s check
in `agent/tools/tech_detector.py`, alongside the existing substring check, and
removed both `@pytest.mark.xfail(strict=True, ...)` markers from
`tests/unit/test_tech_detector.py` on `test_node_modules_excluded` and
`test_build_directory_excluded`.

All test-plan steps passed as predicted: the repro script now returns
`primary_language: 'Python'` (previously `'JavaScript'`); both previously
xfailed tests pass cleanly with no `XPASS(strict)` error; the nested-path
regression case still correctly excludes `src/node_modules/...` files; and
the full 27-test suite passes with no new failures.
