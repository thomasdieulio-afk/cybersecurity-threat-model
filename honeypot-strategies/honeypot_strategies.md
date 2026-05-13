# Honeypot Strategies

Deception-based detection architecture designed to identify adversarial activity
early, slow lateral movement, and gather threat intelligence.

---

## Strategy Overview

The honeypot strategy uses a combination of:
- **Network honeypots** - Fake services that generate alerts on any connection
- **Canary tokens** - Embedded triggers in documents, credentials, and URLs
- **Breadcrumb architecture** - False trails that lure attackers away from real assets

---

## Honeypot Deployments

| Honeypot | Type | Location | Purpose |
|---|---|---|---|
| Fake SSH Server | Network | DMZ | Detect external scanning |
| Canary Admin Account | Credential | AD / IAM | Detect credential stuffing |
| Canary Document | File-based | File share | Detect insider access |
| Fake Database | Network | Internal VLAN | Detect lateral movement |
| Canary URL Token | Web | Login page | Detect credential harvesting |

---

## Alert Triggers

Any interaction with the following generates an immediate high-priority alert:

1. Login attempt on canary admin accounts
2. Any connection to honeypot network services
3. Opening of canary documents (phone-home via token)
4. DNS resolution of canary domains

---

## Canary Token Resources

- [canarytokens.org](https://canarytokens.org) - Free hosted canary tokens
- [thinkst/canarytokens](https://github.com/thinkst/canarytokens) - Self-hosted option

---

## Placement Diagram

> See `/diagrams` for honeypot placement architecture.
