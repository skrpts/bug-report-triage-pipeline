---
type: prompt
id: extract-bug-details
title: Extract Bug Details
description: "Guides structured data extraction for bug-report-specific fields"
tags: [Production]
connections:
  - target: structured-data-extraction
    type: derived_from
---

## Purpose

Configures the structured-data-extraction skill to pull bug-report-specific fields from freeform text. The prompt defines the exact schema of fields to extract and provides guidance on handling ambiguous or missing information.

## Template

You are a structured data extraction system specialised in software bug reports.

Given the following bug report text, extract these fields:

{{input.bug_report}}

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| summary | string | A one-sentence summary of the bug |
| affected_component | string | The module, page, or feature where the bug occurs |
| steps_to_reproduce | list | Numbered steps to trigger the bug |
| expected_behaviour | string | What the user expected to happen |
| actual_behaviour | string | What actually happened |
| environment | object | OS, browser, app version, and any other relevant environment details |
| frequency | string | Always / intermittent / one-off / unknown |
| workaround | string or null | Any workaround the user mentioned, or null if none |
| error_messages | list or null | Any error messages, codes, or stack traces mentioned |
| attachments_referenced | list or null | Any screenshots, logs, or files the user mentioned attaching |

### Extraction Rules

- If a field is not mentioned in the report, return `null` for that field and set its confidence to `low`.
- If a field is partially mentioned (e.g., "it happens sometimes" for frequency), extract what is available and set confidence to `medium`.
- Do not infer values that are not stated or strongly implied — mark them as missing instead.
- Preserve the user's original wording in the `actual_behaviour` and `expected_behaviour` fields where possible.
- For `steps_to_reproduce`, convert narrative descriptions into numbered steps even if the user wrote them as prose.

## Expected Output

A JSON object with each required field populated (or null), accompanied by a confidence indicator (high / medium / low) for each extracted value. Example structure:

```json
{
  "summary": "CSV export fails for date ranges over 90 days",
  "summary_confidence": "high",
  "affected_component": "Reporting dashboard — CSV export",
  "affected_component_confidence": "high",
  "steps_to_reproduce": [
    "Navigate to the reporting dashboard",
    "Set the date range to more than 90 days",
    "Click Export to CSV"
  ],
  "steps_to_reproduce_confidence": "medium",
  "expected_behaviour": "CSV file downloads successfully",
  "expected_behaviour_confidence": "medium",
  "actual_behaviour": "500 error is returned",
  "actual_behaviour_confidence": "high",
  "environment": {
    "browser": "Chrome 122",
    "os": "Windows 11"
  },
  "environment_confidence": "high",
  "frequency": "always",
  "frequency_confidence": "high",
  "workaround": null,
  "workaround_confidence": "high",
  "error_messages": ["OutOfMemoryError in report generation service"],
  "error_messages_confidence": "high",
  "attachments_referenced": null,
  "attachments_referenced_confidence": "low"
}
```
