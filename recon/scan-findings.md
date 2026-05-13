# Reconnaissance & Enumeration — Scan Findings

**Target Environment:** Tempus Fugit / MegaQuagga Lab
**Domain:** `megaquagga.local`
**Toolset:** MegaQuagga Scanning & Enumeration Worksheet
**Raw Output File:** [`MegaQuagga_Scan_Output.xlsx`](MegaQuagga_Scan_Output.xlsx)

> All scanning was performed in a controlled lab environment against systems within scope.

---

## Discovered Infrastructure

| Role | Hostname | IP Address | Notes |
|---|---|---|---|
| Domain Controller / AD | `AD01` | `10.170.0.10` | LDAP on port 389; WMI accessible via PowerShell remoting |
| Web Server | `observer.megaquagga.local` | `10.170.0.20` | HTTPS (443) + HTTP; ffuf directory fuzzing target |

**Domain:** `megaquagga.local`

---

## Scan Categories & Commands Run

### 1 - Network Discovery

| Task | Command / Method |
|---|---|
| Device type identification | Manual classification |
| Network name mapping | Manual / passive recon |
| IP address enumeration | Ping scan |
| Hostname resolution | DNS / NetBIOS |
| OS fingerprinting | Nmap aggressive scan (-A) |
| Service identification | Nmap service/version detection (-sV) |
| MAC address capture | ARP / Nmap |

---

### 2 - Ping / Host Discovery

| Scan Type | Command |
|---|---|
| ICMP Ping Scan (subnet) | `sudo nmap -PE -sn <subnet>` |

---

### 3 - Nmap Scanning

| Scan Type | Command | Purpose |
|---|---|---|
| Aggressive scan (Zenmap) | `Zenmap -T4 -A -v` | OS detection, version, scripts, traceroute (GUI) |
| Aggressive scan (CLI) | `sudo nmap -A <target>` | Full aggressive: OS, version, scripts, traceroute |
| Service and version detection | `nmap -sV <target>` | Identify running services and versions |
| High intensity version scan | `sudo nmap -sV --version-intensity 5 <target>` | Deep service probing |
| Low intensity version scan | `sudo nmap -sV --version-intensity 0 <target>` | Lightweight service probing |
| Script + version scan | `sudo nmap -sV -sC <target>` | Default NSE scripts + version detection |
| SSL Heartbleed check | `nmap -sV -p 443 --script=ssl-heartbleed.nse <target>` | Test HTTPS for CVE-2014-0160 |
| SMB script sweep | `sudo nmap -sV --script=smb* <target>` | All SMB-related NSE scripts |
| HTTP title grab | `sudo nmap --script=http-title <target>` | Enumerate web page titles |
| HTTP headers grab | `sudo nmap --script=http-headers <target>` | Dump HTTP response headers |
| Port scan before fix | `sudo nmap -sS -T4 <target>` | SYN stealth scan - pre-hardening baseline |
| Port scan after fix | `sudo nmap -sS -T4 <target>` | SYN stealth scan - post-hardening comparison |

---

### 4 - SMB Enumeration

| Scan Type | Command | Purpose |
|---|---|---|
| NetBIOS name lookup | `nmblookup -A <target>` | Resolve NetBIOS names for target |
| NetBIOS scan (subnet) | `nbtscan <subnet>` | Enumerate all NetBIOS names on subnet |
| NBstat via Nmap | `nmap --script nbstat.nse <target>` | Pull NetBIOS stats via NSE |
| OS discovery via SMB | `nmap --script smb-os-discovery <target>` | Identify OS through SMB protocol |
| Share enumeration | `nmap --script smb-enum-shares -p139,445 <target>` | List accessible SMB shares |
| SMB version before fix | `nmap -p445 --script smb-protocols <target>` | Detect SMB v1/v2/v3 - pre-hardening |
| SMB version after fix | `nmap -p445 --script smb-protocols <target>` | Detect SMB versions - post-hardening |

**Key ports:** 139 (NetBIOS), 445 (SMB)

---

### 5 - WMI / PowerShell Remoting (AD01)

All commands executed remotely against `AD01` (`10.170.0.10`) via PowerShell `Invoke-Command`.

| Task | Command |
|---|---|
| Test WMI connectivity | `Test-WSMan` |
| Get network config | `Invoke-Command -ComputerName AD01 -ScriptBlock {ipconfig}` |
| Enumerate user accounts | `Invoke-Command -ComputerName AD01 -ScriptBlock {Get-WmiObject -Class Win32_UserAccount}` |
| Find lsass.exe process | `Invoke-Command -ComputerName AD01 -ScriptBlock {Get-WmiObject -Class Win32_Process -Filter 'name="lsass.exe"'}` |
| Enumerate all processes | `Invoke-Command -ComputerName AD01 -ScriptBlock {Get-CimInstance -ClassName Win32_Process}` |

> **Threat Model Link:** lsass.exe enumeration directly validates the **T1003 Credential Dumping** attack path. Confirming lsass.exe is running and WMI-accessible confirms the Mimikatz attack chain is feasible against AD01.

---

### 6 - LDAP Enumeration

| Task | Command |
|---|---|
| Enumerate all user objects | `ldapsearch -x -H ldap://10.170.0.10 -b "dc=megaquagga,dc=local" "(objectclass=user)"` |

> **Threat Model Link:** Successful LDAP enumeration exposes the full AD user list, enabling targeted spearphishing (TA0001) and credential spraying attacks against identified accounts.

---

### 7 - Web Directory Fuzzing (ffuf)

**Target:** `https://10.170.0.20` / `observer.megaquagga.local`
**Wordlist:** `/usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt`

| Scan Type | Command |
|---|---|
| Full fuzz (all responses) | `ffuf -u https://10.170.0.20/FUZZ -w /usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt` |
| Filtered fuzz (interesting codes) | `ffuf -u https://10.170.0.20/FUZZ -w /usr/share/seclists/Fuzzing/fuzz-Bo0oM.txt -mc 200,401,301,302,307` |

**Filtered status codes:** 200 (OK), 401 (Auth required), 301/302/307 (Redirects)

> **Threat Model Link:** Directory fuzzing surfaces hidden admin panels and API endpoints, mapping to **T1190 Exploit Public-Facing Application** and **T1204 User Execution**.

---

### 8 - Web Banner & Signature Grabbing

| Task | Command | Timing |
|---|---|---|
| Website discovery | GUI browser visit | - |
| HTTP banner grab | `curl --head http://observer.megaquagga.local` | Before fix |
| HTTP banner grab | `curl --head http://observer.megaquagga.local` | After fix |
| Webserver signature check | GUI browser inspection | Before fix |

---

## MITRE ATT&CK Mapping

| Technique ID | Name | Scan Activity |
|---|---|---|
| T1595.001 | Active Scanning: Scanning IP Blocks | Nmap ping scan, aggressive scan, port scan |
| T1595.002 | Active Scanning: Vulnerability Scanning | ssl-heartbleed.nse, smb-protocols, smb* scripts |
| T1592 | Gather Victim Host Information | OS detection, service/version detection, HTTP headers |
| T1590.005 | Gather Victim Network Info: IP Addresses | Ping scan, Nmap host discovery |
| T1018 | Remote System Discovery | Nmap subnet sweep, nbtscan, nmblookup |
| T1135 | Network Share Discovery | smb-enum-shares |
| T1069 | Permission Groups Discovery | Win32_UserAccount, LDAP user objects |
| T1057 | Process Discovery | Win32_Process, CimInstance, lsass.exe enumeration |
| T1083 | File and Directory Discovery | ffuf directory fuzzing |
| T1016 | System Network Configuration Discovery | ipconfig via WMI |

---

## Key Findings

| Finding | Severity | Detail |
|---|---|---|
| LDAP enumeration successful | High | Full user list extracted from megaquagga.local - enables targeted phishing and credential spraying |
| lsass.exe confirmed running and WMI-accessible | High | Validates T1003 credential dumping attack path via Mimikatz |
| SMB protocol version exposure | Medium | SMB v1 potentially present pre-fix - vulnerable to EternalBlue-style attacks |
| Web server signature disclosure | Medium | Server version leaked in HTTP headers before hardening |
| Hidden web directories discovered | Medium | Undisclosed endpoints found at 10.170.0.20 via ffuf |
| SSL Heartbleed tested | Low-Medium | Port 443 on observer.megaquagga.local tested for CVE-2014-0160 |
| NetBIOS exposure | Low | NetBIOS names and MAC addresses enumerable |

---

## Hardening Validation (Before vs After)

| Vector | Before Fix | After Fix | Status |
|---|---|---|---|
| Web server signature in HTTP headers | Version disclosed | Headers sanitized | Remediated |
| SMB protocol versions | Legacy SMB exposed | SMBv1 disabled | Remediated |
| Port scan (single host) | Multiple open ports | Reduced attack surface | Remediated |

---

## Files in This Folder

| File | Description |
|---|---|
| `scan-findings.md` | This document - structured findings from all scan categories |
| `MegaQuagga_Scan_Output.xlsx` | Raw tool output - 39 scan columns + LDAP user dump (upload manually) |

---

*MITRE ATT&CK Reference: https://attack.mitre.org/*
