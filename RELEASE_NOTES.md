# Release Notes

## v1.1.27
GH#1070 — the format-conversion step could not run. It now runs, and converts the triage data into a readable report.

The step was declared with a step type the engine does not dispatch, so it failed instead of converting. It now runs the shared format-conversion skill the same way the code-review, documentation and sprint-wrapup pipelines already run it, with the target format declared on the step (`markdown`) rather than left for the skill to guess — the skill supports six formats, and this step needs the one that renders the triage report.

**This skrpt is still hidden from browse and search.** Its PII-masking step cannot run yet — it fails rather than processing anything unmasked, so nothing is mishandled, but the pipeline cannot complete. That needs a deterministic masking step the engine does not have yet (GH#1064). It is now the only step still failing. If you have it installed, keep it: the fix will reach you as a normal update when that lands.

## v1.1.26
GH#1067 — the content-sanitising step could not run. It now runs, and keeps all five of its protections.

The defang step was declared with a step type the engine does not dispatch, so it failed instead of sanitising. It now runs the same way the code-review and documentation pipelines already run it, neutralising dangerous URLs, IP addresses and email addresses, wrapping code-execution patterns in warning markers, and flagging sensitive file paths.

**This skrpt is still hidden from browse and search.** Its PII-masking step cannot run yet — it fails rather than processing anything unmasked, so nothing is mishandled, but the pipeline cannot complete. That needs a deterministic masking step the engine does not have yet (GH#1064). If you have it installed, keep it: the fix will reach you as a normal update when that lands.

## v1.1.25
GH#845 — republish with American English (en-US) content, completing the source-only GH#805 flip that never reached the Hub. Copy only — no functional or behaviour change.

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
Initial catalog release with full structural and content-quality validation. All scanner checks pass.
