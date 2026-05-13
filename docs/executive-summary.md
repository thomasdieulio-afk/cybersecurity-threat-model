# Executive Summary

**Organization:** Tempus Fugit (online gaming platform)
**Author:** Thomas DiEulio
**Date:** May 2026
**Deadline for all controls:** October 29, 2026

---

## Purpose

This threat model was developed to systematically identify and assess cybersecurity risks, document adversary profiles, enumerate attack vectors, and define a layered set of mitigations and detection strategies for the Tempus Fugit gaming environment.

---

## Scope

| Key Asset | Description |
|---|---|
| Intellectual Property (IP) | Proprietary game source code |
| Payment Information | User billing data from paid subscriptions |
| In-Game Private Chat System | End-to-end encrypted player messaging |

---

## Key Findings

1. **File Server and Payment Database are at maximum risk** - Both scored a 9/10 risk rating (Likelihood: 4 - Likely; Impact: 5 - Catastrophic). Unauthorized access, SQL injection, credential dumping, and ransomware are all credible and high-impact threats.

2. **Credential dumping (T1003) is the highest-priority technical threat** - Mimikatz-style LSASS extraction would allow attackers to obtain domain admin credentials, move laterally across the IT_Network subnet, and reach both the file server and payment database without triggering traditional defenses.

3. **Spearphishing is the most likely initial access vector (TA0001)** - Employees are the primary entry point. A successful phishing email leading to credential capture enables the full attack chain: initial access to lateral movement to credential dumping to exfiltration.

4. **The in-game chat system is a high-risk user execution surface (T1204)** - Scored 8/10 risk. Attackers can weaponize chat messages to deliver malicious links and files directly to players, enabling malware infections and account compromise at scale.

5. **Insider threat is a cross-cutting risk across all three assets** - Insiders with legitimate access can exfiltrate source code, misuse billing records, and abuse chat moderation tools without triggering standard security controls, making detection-first strategies (canary tokens, behavioral monitoring) essential.

---

## Risk Posture

| Category | Risk Level | Notes |
|---|---|---|
| External Threat Actors | High | Nation-state actors, APTs, cybercriminals all actively target gaming IP and payment data |
| Attack Surface Exposure | High | File server (9/10), Payment DB (9/10), Chat server (8/10) |
| Detection Coverage | Medium | SIEM and EDR partially deployed; gaps in credential dumping and lateral movement detection |
| Mitigation Maturity | Low-Medium | MFA, Credential Guard, EDR, and canary tokens not yet fully deployed; deadline Oct 29, 2026 |

---

## Threat Actor Summary

| Threat Actor | Primary Target | Capability | Top TTP |
|---|---|---|---|
| Insider Threats | IP, Payment Info, Chat | Medium | T1052 Exfiltration Over Physical Medium |
| Nation-State Actors | Intellectual Property | High | T1135 Network Share Discovery + T1041 C2 Exfiltration |
| APTs | IP, Payment Info | High | T1003 Credential Dumping |
| Cybercriminals | Payment Info, IP | Medium-High | T1190 Exploit Public-Facing App + T1204 User Execution |
| Script Kiddies | Chat System | Low | T1498 Network DoS |

---

## Recommended Immediate Actions (Priority 1)

1. **Deploy MFA** on all external-facing systems - blocks 99.9% of automated credential attacks (Microsoft/Verizon DBIR)
2. **Enable Credential Guard** on all domain-joined endpoints - prevents Mimikatz/LSASS-based credential dumping (T1003)
3. **Enforce email security (DMARC/DKIM/SPF)** - closes the primary spearphishing initial access vector (TA0001)
4. **Deploy canary credentials and documents** - provides immediate early warning for insider threats and lateral movement with zero false positives

---

## Goals and Deadlines

| Goal | Metric | Deadline |
|---|---|---|
| Prevent credential theft and detect unauthorized credential use | Reduce successful unauthorized logins by 80% | October 29, 2026 |
| Detect 100% of credential dumping attempts | EDR + SIEM alerts on all LSASS access events | October 29, 2026 |
| Reduce malicious user execution on endpoints | Reduce unauthorized file execution by 75% | October 29, 2026 |

---

## Stakeholder Impact

| Stakeholder | Solution | Why It Matters |
|---|---|---|
| IT Security Team | MFA + Credential Guard + EDR + SIEM | Credential dumping can lead to full domain compromise if not prevented and detected |
| IT Security Team | Application Allowlisting + Email Filtering + Training | Malicious user execution is the primary malware entry point - human interaction drives 70%+ of breaches (Verizon DBIR) |

---

*Full details available in:*
- Threat_Model_Report_Thomas_DiEulio.docx
- Threat_Modeling_Worksheet_Thomas_DiEulio.xlsx
