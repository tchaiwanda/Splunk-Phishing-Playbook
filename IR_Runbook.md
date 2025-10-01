# IR Runbook — Phishing Triage (one page)

Step 0 — Intake
- Create ticket; assign SOC analyst.
- Capture full email (headers + body).

Step 1 — Quick Triage (0–15min)
- Inspect headers: SPF/DKIM/DMARC results.
- Sandbox attachments/URLs (if available).
- Search SIEM for same subject/sender/client.
- Classify: False / Phish / Suspicious.

Step 2 — Contain (15–60min)
- Quarantine email across mail gateway.
- Block sender domain & IPs.
- Block URLs at web proxy / DNS.
- Disable affected accounts (if evidence of compromise).

Step 3 — Eradicate (1–8h)
- Reset passwords/revoke tokens.
- Run EDR scan, remove persistence.

Step 4 — Recover & Monitor (1–30d)
- Re-enable access after validation.
- Monitor accounts for 7–30 days.

Step 5 — Post Incident
- Document timeline & lessons.
- Update SIEM detections.
- Run mini-awareness training if necessary.