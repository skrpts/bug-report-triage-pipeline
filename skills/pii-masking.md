---
type: skill
id: pii-masking
title: PII Masking
description: "Detects and masks personally identifiable information before content is processed or stored"
tags: [Production, Security, Privacy]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Scans text for personally identifiable information (PII) and replaces it with safe placeholders before the content is passed to downstream stages. Prevents accidental exposure of personal data in AI processing, logs, or outputs.

## What It Detects

1. **Names** — full names, first/last names in context
2. **Email addresses** — any format
3. **Phone numbers** — international and local formats
4. **Physical addresses** — street addresses, postcodes
5. **National identifiers** — National Insurance numbers, SSNs, passport numbers
6. **Financial data** — credit card numbers, bank account numbers, sort codes
7. **IP addresses** — IPv4 and IPv6
8. **Dates of birth** — when identifiable as DOB in context

## Masking Format

Each PII item is replaced with a typed placeholder: `[NAME-1]`, `[EMAIL-1]`, `[PHONE-1]`, etc. The numbering preserves identity relationships (the same name always maps to the same placeholder within a document).

## When to Use

- Before processing customer support tickets or bug reports
- Before sending user-submitted content to an AI provider
- On research data containing participant information
- Any time raw user input enters a pipeline

## Inputs

Text content that may contain PII.

## Outputs

Masked text with PII replaced by typed placeholders. Optionally, a mapping table for re-identification (stored locally, never sent to AI providers).
