---
type: workflow
id: bug-report-triage-pipeline
title: Bug Report Triage Pipeline
description: "Classifies incoming bug reports, extracts structured details, and triages by severity"
tags: [Production, Automation]
connections:
  - target: intent-classification
    type: uses
  - target: structured-data-extraction
    type: uses
  - target: llm-service
    type: runs_on
  - target: format-conversion
    type: uses
  - target: pii-masking
    type: uses
  - target: defang-content
    type: uses
output_step: "structured-data-extraction"
composite_steps:
  - "intent-classification"
  - "structured-data-extraction"
  - "format-conversion"
  - "pii-masking"
  - "defang-content"
execution:
  - skill: "intent-classification"
    step_type: "synthesis"
  - skill: "structured-data-extraction"
    step_type: "synthesis"
  - skill: "format-conversion"
    step_type: "content"
  - skill: "pii-masking"
    step_type: "content"
  - skill: "defang-content"
    step_type: "content"
---

## Overview

This workflow processes incoming bug reports through a three-stage pipeline. It first classifies the report's intent to confirm it is genuinely a bug (rather than a feature request or question), then extracts structured details such as affected component, reproduction steps, and environment information. Finally, it cross-references the extracted data against the incident response plan to assign a severity level and recommend next actions.

## Pipeline Stages

### Stage 1: Classify Report Intent

Invoke the **intent-classification** skill using the **bug-report-triage** prompt to analyse the incoming report. The classifier determines whether the submission is a genuine bug report, a feature request, a support question, or something else entirely. Reports that are not bugs receive a redirect recommendation rather than entering the triage queue.

The raw report text is passed as `{{input.bug_report}}`.

### Stage 2: Extract Structured Details

Using `{{steps.Classify Intent.output}}` to confirm the report is a valid bug, invoke the **structured-data-extraction** skill with the **extract-bug-details** prompt. This stage pulls out key fields from the freeform text:

- Affected component or module
- Steps to reproduce
- Expected versus actual behaviour
- Environment details (OS, browser, version)
- Frequency and reproducibility
- Any attached logs or screenshots referenced

The extraction result is available as `{{steps.Structured Data Extraction.output}}`.

### Stage 3: Severity Assignment and Triage

Combine `{{steps.Classify Intent.output}}` and `{{steps.Structured Data Extraction.output}}` with the **incident-response-plan** source to determine the appropriate severity level (P1 through P4). The triage logic considers:

- User impact scope (single user vs. widespread)
- Whether a workaround exists
- Data integrity or security implications
- Service availability impact

The final triage decision is available as `{{steps.Severity Assignment.output}}`.

## Output

A structured triage report containing:

- Confirmed intent classification (bug, feature request, question, or other)
- Extracted bug details in a consistent, structured format
- Assigned severity level (P1-P4) with justification
- Recommended response time based on the incident response plan
- Suggested assignee team or escalation path
- Any immediate actions required (e.g., security team notification for P1)

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.bug_report}}` | Yes | The raw bug report text submitted by the user | "Login page crashes on Safari 17 when using SSO. Console shows TypeError: Cannot read properties of undefined. Happens every time." |

## Outputs

| Name | Description |
|------|-------------|
| Intent classification | Whether the submission is a bug report, feature request, question, or other |
| Structured bug details | Extracted fields including component, reproduction steps, environment, and frequency |
| Severity assignment | P1-P4 rating with justification and recommended response time |
| Triage recommendation | Suggested team assignment, escalation path, and immediate actions |

## Setup

Before running this workflow:

1. **Paste the bug report text** — no external integrations are required. Simply provide the raw text of the bug report as submitted by the user.
2. **Review the incident response plan** — the source document defines your organisation's severity levels and response expectations. Customise it to match your actual SLAs and team structure.

No specific AI provider or API key is required beyond your configured skrptiq LLM provider.

## Provider Notes

- Works well with any model — no specific provider requirements
- Classification and extraction tasks are handled reliably by most LLMs
- Models with stronger reasoning capabilities produce more accurate severity assessments, particularly for edge cases where the impact is not immediately obvious from the report text
- Extraction, classification, and triage steps generally run well on smaller or faster models

## Example Input

To test this workflow immediately after import:

```
Bug report: "The export to CSV function in the reporting dashboard throws a 500 error when the date range exceeds 90 days. This affects all users on the Pro plan who need quarterly reports. No workaround exists — they cannot export data at all for ranges over 90 days. Error started appearing after the v2.4.1 release last Tuesday. Browser: Chrome 122, OS: Windows 11. Server logs show an OutOfMemoryError in the report generation service."
```
