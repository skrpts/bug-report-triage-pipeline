# Release Notes

## v1.1.24
GH#745 — declare per-step `output: {name, type}` on every execution step (classification/text, triage_data/json, formatted_report/text, masked_report/text, sanitised_report/text). Lights up the #744 rich flow-map. Content-only; no bindings or logic changes.

## v1.1.23
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 6 inline shared-content files and declare 6 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Closes pre-Step-3 inline-vendoring for this bundle.

## v1.1.22
Added missing manifest ID (GH#555 compliance).

## v1.1.21
Wave 2: re-signed with canonical engine signing pipeline.

## v1.1.20
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.1.19
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.1.18
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.1.17
Initial catalogue release with full structural and content-quality validation. All scanner checks pass.
