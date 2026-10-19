# Release builder state

OA-05 adds a candidate v2.0.0-rc.1 source bundle from exact Git SHAs and a
reviewed SHA-256 source lock. The 2026-10-19 snapshot is for reproducibility
validation; it is not the final release selection or acceptance evidence.

The v0.0.1 archive-input path remains available. The rc.1 path uses the same
build and validation entry points and preserves workspace checkout paths.
Source archive checksums use canonical, uncompressed ZIPs produced from verified
Git commits. Output ZIP metadata and ordering are deterministic.

Overlaps: PR #9 adds navigation installation validation to the legacy builder;
PR #8 documents the release model. Their changes are not adopted or superseded
by this work. The release-local snapshot will need alignment with the OA-39
manifest after its review. OA-06 driver installation is a separate work item.

No safety implementation is changed. Source bundling does not validate package
builds, firmware, robot operation, physical safety or release readiness.
Upstream build-time downloads are not locked by this source bundle.
