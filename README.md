# 🛡️ Threat Model — Thomas DiEulio

> A structured cybersecurity threat model documenting threat actors, attack surfaces,
> honeypot strategies, detection techniques, and mitigations for a defined target environment.

![Status](https://img.shields.io/badge/status-active-brightgreen)
![Security](https://img.shields.io/badge/domain-cybersecurity-blue)
![Type](https://img.shields.io/badge/type-threat--model-orange)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Folder Structure](#folder-structure)
- [Threat Actors](#threat-actors)
- [Attack Surface](#attack-surface)
- [Recon & Enumeration](#recon--enumeration)
- [Honeypot Strategies](#honeypot-strategies)
- [Detection Strategies](#detection-strategies)
- [Mitigations](#mitigations)
- [Full Report](#full-report)
- [Author](#author)

---

## Overview

This repository contains a comprehensive threat model developed to identify, analyze, and
prioritize security threats against a target environment. The model follows a structured
methodology covering adversary profiling, surface enumeration, active reconnaissance,
deception tactics, detection engineering, and layered mitigation planning.

The threat model is intended to serve as a living document — updated as the environment
evolves, new threat intelligence emerges, or controls are implemented.

---

## Folder Structure

| Folder | Description |
|---|---|
| `docs/` | Full threat model report (.docx), worksheet (.xlsx), and executive summary |
| `threat-actors/` | Profiles of identified adversaries and threat groups |
| `attack-surface/` | Enumeration of entry points, assets, and exposure areas |
| `recon/` | Active scanning and enumeration findings (Nmap, SMB, LDAP, ffuf, WMI) |
| `honeypot-strategies/` | Deception and canary-based detection architectures |
| `detection-strategies/` | SIEM rules, alerting logic, and behavioral indicators |
| `mitigations/` | Prioritized controls mapped to identified threats |
| `diagrams/` | Architecture and threat flow diagrams |
| `scripts/` | Supporting automation and analysis scripts |

---

## Threat Actors

Profiles of threat actors relevant to this environment, including motivation,
capability level, and known TTPs (Tactics, Techniques, and Procedures).

📄 See [`threat-actors/threat_actors.md`](threat-actors/threat_actors.md)

---

## Attack Surface

A mapped enumeration of all identified attack vectors, exposed services, network
entry points, and asset categories within scope.

📄 See [`attack-surface/attack_surface.md`](attack-surface/attack_surface.md)

---

## Recon & Enumeration

Active scanning and enumeration findings from the MegaQuagga lab environment, covering
39 scan categories across Nmap, SMB, LDAP, WMI, ffuf, and web banner analysis. Includes
before/after hardening comparisons and full MITRE ATT&CK technique mapping.

📄 See [`recon/scan-findings.md`](recon/scan-findings.md)

---

## Honeypot Strategies

Deception-based detection strategies including honeypot placement, canary token
deployment, and breadcrumb architecture designed to detect and slow adversaries.

📄 See [`honeypot-strategies/honeypot_strategies.md`](honeypot-strategies/honeypot_strategies.md)

---

## Detection Strategies

Behavioral detection logic, SIEM correlation rules, alerting thresholds, and
indicator-of-compromise (IOC) patterns aligned to the identified threat actors.

📄 See [`detection-strategies/detection_strategies.md`](detection-strategies/detection_strategies.md)

---

## Mitigations

Prioritized security controls and countermeasures mapped to each threat category,
including recommended implementation sequencing and ownership.

📄 See [`mitigations/mitigations.md`](mitigations/mitigations.md)

---

## Full Report

The complete threat model report and supporting documents are available in the `docs/` directory:

📎 [`docs/Threat_Model_Report_Thomas_DiEulio.docx`](docs/Threat_Model_Report_Thomas_DiEulio.docx)
📊 [`docs/Threat_Modeling_Worksheet_Thomas_DiEulio.xlsx`](docs/Threat_Modeling_Worksheet_Thomas_DiEulio.xlsx)

---

## Author

**Thomas DiEulio**
Cybersecurity | SOC Analysis | Threat Intelligence

---

*This threat model is for authorized security assessment and planning purposes only.*
