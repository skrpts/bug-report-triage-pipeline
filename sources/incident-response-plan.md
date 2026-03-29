---
type: source
id: incident-response-plan
title: Incident Response Plan
description: "Reference document defining severity levels, response procedures, and escalation paths for production incidents"
tags: [Production, quality:security, utility:classification]
connections: []
---

## Purpose

This document defines the severity classification system, response time expectations, escalation procedures, and communication templates used during bug triage and incident response. All triage decisions should reference these definitions to ensure consistency across the organisation.

## Severity Levels

### P1 — Critical

**Definition:** Complete service outage, data loss or corruption, security breach, or a bug affecting all users with no workaround available.

**Response time:** Acknowledge within 15 minutes. Begin investigation immediately. Status update to stakeholders every 30 minutes until resolved.

**Resolution target:** 4 hours.

**Examples:**
- Authentication system is down — no users can log in
- Database corruption causing data loss across accounts
- Payment processing returning incorrect amounts
- Security vulnerability actively being exploited
- Core API returning 500 errors on all endpoints

**Escalation:** Automatically escalates to the on-call engineering lead, Head of Engineering, and CTO. Security incidents additionally notify the security team and compliance officer.

### P2 — High

**Definition:** Major feature broken or degraded for a significant subset of users. A workaround may exist but is unreasonable for sustained use.

**Response time:** Acknowledge within 1 hour. Begin investigation within 2 hours. Status update to stakeholders every 2 hours.

**Resolution target:** 24 hours.

**Examples:**
- CSV export fails for date ranges over 90 days (affects all Pro plan users needing quarterly reports)
- Search returns incomplete results for queries containing special characters
- Mobile app crashes on launch for devices running Android 14
- Webhook delivery failing for approximately 30% of configured endpoints

**Escalation:** Assigned to the relevant team lead. If unresolved after 12 hours, escalates to the Head of Engineering.

### P3 — Medium

**Definition:** Feature partially broken or behaving incorrectly, but a reasonable workaround exists. Affects a limited subset of users or a non-critical feature.

**Response time:** Acknowledge within 4 hours. Begin investigation within one business day. Status update to stakeholders daily.

**Resolution target:** 1 week.

**Examples:**
- Tooltip text displays the wrong message on the settings page
- Sorting by date on the dashboard occasionally shows items out of order
- PDF export includes an extra blank page at the end
- Dark mode colours are incorrect on the notifications panel

**Escalation:** Assigned to the relevant team's backlog. If not picked up within 3 business days, the team lead reviews and reprioritises.

### P4 — Low

**Definition:** Minor cosmetic issue, edge case with negligible user impact, or a suggestion for improvement that surfaced through the bug report channel.

**Response time:** Acknowledge within one business day. Schedule for a future sprint.

**Resolution target:** Best effort — typically within the current quarter.

**Examples:**
- Alignment slightly off on the footer in Firefox at a specific viewport width
- Placeholder text in an input field has a minor grammatical error
- Hover state on a button is a slightly different shade than the design specification
- An unused API parameter is still accepted without returning a deprecation warning

**Escalation:** Added to the team backlog. No escalation required unless the volume of P4 issues indicates a systemic quality problem.

## Triage Criteria

When determining the severity of a bug report, evaluate these factors in order:

1. **Data integrity** — Is any data at risk of being lost, corrupted, or exposed? If yes, this is at minimum P2. If data loss is actively occurring, P1.

2. **Service availability** — Is the affected feature entirely unusable? If the core product is down, P1. If a major feature is down, P2. If a minor feature is down with a workaround, P3.

3. **User impact scope** — How many users are affected? Organisation-wide impact with no workaround is P1. A significant segment (e.g., all users on a specific plan or platform) is P2. A small subset or edge case is P3 or P4.

4. **Workaround availability** — Does a reasonable workaround exist? "Reasonable" means a non-technical user can follow it without significant effort. If the workaround is complex or fragile, treat it as if no workaround exists.

5. **Security implications** — Any bug with security implications (information disclosure, privilege escalation, injection vectors) is at minimum P2, regardless of how few users are affected.

6. **Regression** — If the bug was introduced in a recent release (regression), increase the severity by one level. Regressions indicate a gap in testing that may affect other areas.

## Escalation Paths

| Severity | First responder | Escalation after | Escalated to |
|----------|----------------|-------------------|--------------|
| P1 | On-call engineer | Immediate | Engineering lead, Head of Engineering, CTO |
| P2 | Team lead | 12 hours | Head of Engineering |
| P3 | Team backlog | 3 business days | Team lead |
| P4 | Team backlog | End of quarter | Team lead (batch review) |

### Cross-team escalation

If a bug spans multiple teams (e.g., a frontend issue caused by a backend API change), the triage owner assigns a primary team and notifies the secondary team. The primary team owns the fix; the secondary team provides support and review.

### External escalation

Bugs reported by enterprise customers or those affecting contractual SLAs follow the same severity framework but include the Customer Success team in all status updates from P2 upward.

## Communication Templates

### Acknowledgement (all severities)

> Thank you for reporting this issue. We have logged it as [ticket ID] with severity [P1/P2/P3/P4]. Our team will investigate and provide an update within [response time per severity]. If you have additional information that might help us reproduce or diagnose the issue, please reply to this thread.

### Status update (P1 and P2)

> **Status update — [ticket ID]**
> **Current status:** Investigating / Identified root cause / Fix in progress / Fix deployed
> **Summary:** [One-sentence description of what is known so far]
> **Impact:** [Current user impact and scope]
> **Next update:** [Time of next scheduled update]
> **Actions taken:** [Brief list of steps completed since last update]

### Resolution notification

> **Resolved — [ticket ID]**
> **Root cause:** [Brief explanation of what caused the issue]
> **Fix:** [What was changed to resolve it]
> **Deployed:** [Timestamp and release version]
> **Monitoring:** We will continue monitoring for [time period] to confirm the fix is stable. If you experience any further issues, please let us know.
