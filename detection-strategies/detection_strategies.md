# Detection Strategies

Behavioral detection logic, SIEM correlation rules, and indicator-of-compromise (IOC)
patterns aligned to the threat actors and attack surface identified in this model.

---

## Detection Philosophy

Detection is layered across three tiers:
1. **Prevention-layer telemetry** - Firewall, EDR, email gateway alerts
2. **Behavioral analytics** - Anomaly detection and baseline deviations
3. **Threat intelligence correlation** - IOC matching against known bad actors

---

## SIEM Correlation Rules

| Rule Name | Trigger Condition | Severity | Response |
|---|---|---|---|
| Brute Force - Admin Account | 10+ failed logins in 5 min | High | Lock + alert |
| Canary Token Access | Any access to canary resource | Critical | Immediate escalation |
| Unusual Outbound Connection | Connection to new external IP on port 443 | Medium | Investigate |
| Lateral Movement Indicator | RDP/SMB from non-admin host | High | Isolate + alert |
| Privilege Escalation Attempt | Sudo / UAC bypass event | High | Alert + log |

---

## Indicators of Compromise (IOCs)

> Populate with IOCs specific to identified threat actors.

| Type | Value | Actor | Confidence |
|---|---|---|---|
| IP Address | x.x.x.x | Actor 1 | High |
| Domain | malicious.example.com | Actor 1 | Medium |
| File Hash (SHA256) | abc123... | Actor 2 | High |

---

## Log Sources

| Source | Data Type | Retention |
|---|---|---|
| Firewall | Network flow, block events | 90 days |
| EDR | Process, file, network events | 180 days |
| SIEM | Aggregated alerts | 1 year |
| Active Directory | Auth events, group changes | 1 year |
| Email Gateway | Phishing attempts, attachments | 90 days |

---

## MITRE ATT&CK Detection Coverage

| Technique | Detection Method | Coverage |
|---|---|---|
| T1566 Phishing | Email gateway + user reports | Medium |
| T1059 Command Execution | EDR process monitoring | High |
| T1071 C2 over HTTP | Proxy / DNS monitoring | Medium |
