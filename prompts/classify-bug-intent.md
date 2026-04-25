---
type: prompt
id: classify-bug-intent
title: "Classify Bug Intent"
description: "Classifies incoming bug reports by intent — bug, feature request, question, or other"
tags: [Production, Automation, Triage]
inputs:
  bug_report:
    label: "Bug Report"
    description: "The bug report to triage — paste the text or upload a file"
    example: "Paste the bug report here"
    required: true
    type: file
    accept: ".txt,.md,.docx"
connections:
  - target: intent-classification
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

You are a support triage specialist. Classify the incoming report and extract initial signals.

### Report

{{input.bug_report}}

### Instructions

1. **Intent classification** — is this a bug report, feature request, question, or other?
2. **Confidence** — how confident are you in the classification (high/medium/low)?
3. **Initial severity signal** — if it's a bug, what severity does it suggest (P1-P4)?
4. **Key indicators** — what in the report led to your classification?
5. **Missing information** — what details would help with triage?

If this is NOT a bug report, classify it and recommend where to redirect it.

### Output

Return a structured classification:

**Intent:** [bug | feature-request | question | other]
**Confidence:** [high | medium | low]
**Severity Signal:** [P1 | P2 | P3 | P4 | N/A]
**Indicators:** [list key phrases/signals]
**Missing:** [list any missing information]
**Recommendation:** [triage action or redirect]
