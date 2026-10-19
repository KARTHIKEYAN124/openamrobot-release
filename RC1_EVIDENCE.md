# OA-05 verification

Candidate snapshot date: 2026-10-19.
Base: `b9f4a9d2a1845ab50769693240a3050cf0f6d171`.
The PR head is recorded in the PR description after committing.

## Regression and fault injection

`python -m unittest discover -s tests -v`: **16 tests, 0 failures** on
Windows with Python 3.14 and Git 2.55.0.windows.5. Tests cover exact SHA fetch
arguments, fetched commit mismatch, submodule rejection, mutable refs,
duplicate keys, changed manifest bytes, changed cache bytes, fetched archive
checksum mismatch, incorrect archive commit, missing checksum coverage,
tampered output, extra output members, unsafe paths, executable/link modes,
ZIP metadata normalization, reproducible output and CLI dispatch.
The legacy v0.0.1 build and validator pass using eight fixture source archives.
Fixture tests establish packaging behavior, not component build acceptance.

Before-change regression: ran `scripts/build_release.py` from base `b9f4a9d`
against `release-config.rc1.json` in a temporary output directory. It exits 1
with `KeyError: 'components'`. The updated entry point accepts the same config.

## Independent real-source builds

Executed separately, each fetching all 14 repositories into a new temporary
Git repository and using separate source caches:

```sh
python scripts/build_release.py --config release-config.rc1.json --input-dir input/first --output-dir dist/first --fetch
python scripts/build_release.py --config release-config.rc1.json --input-dir input/second --output-dir dist/second --fetch
python scripts/validate_release.py dist/first/OpenAMRobot-v2.0.0-rc.1-source.zip
```

Both builds exit 0, each verifies **1,841 files**, and byte comparison using
Python `filecmp.cmp(..., shallow=False)` passes. The bundle is 231,082,934 bytes.

SHA-256 for both bundles:

```text
2f87bf21a99add7fb3aa37b2965943c4ac6fa390054476b5ea5293f6500fed41
```

The SHA inventory and commit provenance are inside each bundle; the external
`.zip.sha256` accompanies it. Generated ZIPs and source caches are not committed.
The new hosted workflow repeats two independent fetches/builds on Ubuntu 24.04
and uploads the validated candidate, without publishing a GitHub Release.

## Not verified

Full Ubuntu/Jazzy workspace compilation, third-party build-time dependency
pinning, firmware builds, drivers on physical hardware and rc.1 acceptance
are outside this source-packaging verification. No hardware or safety claim
is made. Hosted CI status is reported separately from local test results.

## Integration and provenance

PR #9's navigation-install work and PR #8's release-model documentation were
inspected. This change preserves the legacy builder path and keeps the rc.1
procedure in a separate document. No assertion is made about inaccessible work.
The release-local manifest will be aligned with OA-39 after review; it includes
13 workspace repositories plus the release-only governance source.

Prepared with OpenAI Codex: implementation, candidate lock, tests, workflow
and documentation. No safety logic or third-party source was authored or
modified. Third-party source and licence notices remain in their exports;
tooling dependency and upstream licences are described in RC1_RELEASE.md.
