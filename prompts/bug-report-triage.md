---
type: prompt
id: bug-report-triage
title: Bug Report Triage
description: "Combines classified intent, extracted details, and incident response guidelines to assign severity and triage actions"
tags: [Production]
connections:
  - target: intent-classification
    type: derived_from
  - target: structured-data-extraction
    type: derived_from
  - target: incident-response-plan
    type: references
---

## Purpose

Assigns a severity level and triage recommendation to a bug report by combining the classified intent, extracted structured details, and the organisation's incident response plan. This prompt drives the final stage of the pipeline, producing an actionable triage output that can be handed directly to the engineering team.

## Template

You are an experienced software engineering lead responsible for triaging incoming bug reports.

You have three inputs:

1. **Intent classification result:**
{{steps.classify-intent.output}}

2. **Extracted bug details:**
{{steps.extract-details.output}}

3. **Incident response plan:**
Reference the incident response plan for severity definitions, response time expectations, and escalation paths.

Using these inputs, perform the following:

- If the intent classification indicates this is **not a bug report** (e.g., it is a feature request or question), return a redirect recommendation with a brief explanation. Do not assign a severity level.
- If it **is a bug report**, assign a severity level (P1, P2, P3, or P4) based on the extracted details and the criteria defined in the incident response plan.
- Justify your severity assignment by referencing specific details from the bug report (e.g., number of users affected, availability of a workaround, data integrity implications).
- Recommend the appropriate response actions, team assignment, and escalation path per the incident response plan.
- Flag any missing information that would affect the accuracy of the triage (e.g., no reproduction steps, unclear environment details).

Be conservative with severity: when the evidence is ambiguous, assign a higher severity and note the uncertainty. It is better to over-triage and downgrade than to miss a critical issue.

## Expected Output

A structured triage report containing:

- **Intent:** Confirmed classification (bug / feature request / question / other)
- **Severity:** P1, P2, P3, or P4 (or N/A if not a bug)
- **Justification:** 2-3 sentences explaining why this severity was assigned
- **Impact scope:** Single user / team / department / organisation-wide
- **Workaround available:** Yes (with description) / No
- **Recommended actions:** Immediate steps to take
- **Assigned team:** Which team should own the fix
- **Response deadline:** Per the incident response plan SLAs
- **Missing information:** Any gaps that should be clarified with the reporter
