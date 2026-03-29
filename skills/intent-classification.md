---
type: skill
id: intent-classification
title: Intent Classification
description: "Analyses text input and classifies it into intent categories"
tags: [Tested, utility:classification]
connections:
  - target: llm-service
    type: runs_on
---

## Capability

Analyses text input and classifies it into intent categories (question, request, complaint, feedback, etc.).

## When to Use

- Routing customer support messages
- Triaging incoming requests
- Building conversational flows

## Inputs

User message or text input

## Outputs

Intent category + confidence indicator
