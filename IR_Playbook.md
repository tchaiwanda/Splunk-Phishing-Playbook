# Incident Response Playbook — Phishing & Credential Compromise

## Overview
**Purpose:** Provide a concise, repeatable triage and containment workflow for phishing reports and credential compromise events.
**Scope:** End-user reported phishing, detected suspicious email campaigns, and authentication anomalies that indicate credential compromise.

## TL;DR
1. Triage: validate email & scope (who got it, indicators).
2. Contain: quarantine messages, block indicators (URLs, sender, IP).
3. Eradicate: reset compromised credentials, rotate tokens, remove persistence.
4. Recover: restore services and validate.
5. Lessons Learned: update detections and run awareness.

## Detection Sources
- Email gateway logs (Proofpoint, Mimecast, Office 365 ATP) — fields: `sender, recipient, subject, url, verdict, attachment_hash`
- Mail server logs (Exchange/SMTP)
- Endpoint telemetry (EDR)
- Authentication logs (Splunk / Azure AD sign-in)
- User reports / Phish Alert Button

## Triage (0–20 minutes)
**Goal:** quickly validate whether the message is malicious and scope the impact.
1. Gather initial info: ticket ID, reporter, timestamp, full email (headers + body), sender, recipients, subject, attachments, URLs.
2. Initial checks: SPF/DKIM/DMARC, domain reputation, sandbox attachments/URLs.
3. Scope search in SIEM for same message-id/subject/sender and clicks.
4. Classify: Benign / Phish / Unknown.

## Containment (15–60 minutes)
1. Quarantine messages at gateway; delete from mailboxes if supported.
2. Block sender domains/IPs; add IoCs to blocklists.
3. Block malicious URLs at web gateway/DNS.
4. If compromise likely: disable accounts, force password reset, revoke tokens.
5. Isolate affected endpoints and run EDR scans.

## Eradication & Recovery
- Reset credentials, rotate keys, revoke OAuth tokens.
- Remove malicious mail rules, persistence.
- Re-image hosts if required; validate services and monitor for 7–30 days.

## Roles & Responsibilities
- SOC Analyst (Tier 1): Triage, quarantine, create ticket.
- IR Lead (Tier 2): Coordinate containment, approve remediation steps.
- IT Admin: Reset credentials, apply network rules.
- Communications/Legal: Stakeholder notifications.

## Evidence & Artifacts to Collect
- Full email (headers + body), gateway logs, SIEM search results, endpoint artifacts, screenshots.

## Ticket & Communication Templates
(See IR_Runbook.md for concise templates)

## Quick Splunk Checks (examples)
- Find email by subject/sender:
```
index=email sourcetype=mail_gateway subject="*Invoice*" OR subject="*Payment*"
| stats count by sender, recipient, subject
```

- Find link clicks:
```
index=email sourcetype=mail_gateway action=click url="*suspicious-domain.com*"
| stats count by recipient, url, _time
```

- Check auth for a user:
```
index=security sourcetype=auth:csv user=jsmith | table _time, src_ip, action, host
```