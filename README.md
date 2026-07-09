# Cleveland Fintech — Threat Intelligence Investigation Report

A hands-on threat intelligence case study analysing a live **DarkGate malware** sample targeting Cleveland Fintech. This project covers malware fingerprinting, infrastructure mapping, MITRE ATT&CK technique identification, and actionable SOC recommendations.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Malware Profile](#malware-profile)
- [Infrastructure & Network Relations](#infrastructure--network-relations)
- [MITRE ATT&CK Mapping](#mitre-attck-mapping)
- [Executive Summary & Findings](#executive-summary--findings)
- [Tools and Technologies](#tools-and-technologies)
- [Author](#author)

---

## Project Overview

This project documents a structured threat intelligence investigation conducted for **Cleveland Fintech's Security Operations Center (SOC)**. A suspicious executable (`setup.exe`) was submitted for analysis and identified as a **DarkGate Remote Access Trojan (RAT)** — a critical-severity threat capable of backdoor access, credential theft, and establishing covert Command & Control (C2) communications.

The investigation follows a structured methodology covering surface identification, infrastructure mapping, attacker behaviour profiling using the **MITRE ATT&CK framework**, and concrete mitigation recommendations.

---

## Malware Profile

| Attribute | Details |
|---|---|
| **Malicious Family** | DarkGate |
| **File Name** | `setup.exe` |
| **File Type** | EXE (Win32 Executable / peexe) |
| **File Size** | 1.59 MB (1,666,928 bytes) |
| **Date of Creation** | 2025-10-21 14:56:49 UTC |
| **Functions** | Remote Access Trojan (RAT); Backdoor; Information Stealer |

**Digital Fingerprints (Hashes):**

- **SHA256:** `dab701feecc382b037b61b4268f1f796c28f3c30d77e18506cb1646bf9cb0`
- **SHA1:** `f3a403871eee2abf3d4150c3b0dbd878c8c80c31`
- **MD5:** `358e54bf814e5c420568c0af8cd13df9`

**Related Malicious Artifacts:**

- `.rsrc/1033/RCDATA/CABINET`
- `AutoIt3.exe`
- `Palestine_xlm`

---

## Infrastructure & Network Relations

| Attribute | Details |
|---|---|
| **Antivirus Detection Score** | 52 / 71 security vendors |
| **Vendors Flagging as Malicious** | 13 |
| **Malicious IP Addresses** | `176.126.86.247`; `192.121.108.99` |
| **Contacted Domains / C2 URL** | `http://70.36.99.253:15888/gateway/l29mnvj4.0qno9` |

**C2 Behaviour:** The malware uses a custom C2 framework designed to blend into normal web traffic, maintaining persistent access to compromised systems through unmonitored HTTP/HTTPS channels — a technique known as **Non-Standard Port (MITRE T1571)**.

---

## MITRE ATT&CK Mapping

### Behavior Profile 1 — T1027: Obfuscated Files or Information

- **Tactic:** Stealth / Defense Evasion
- **What the malware does:** Encrypts, encodes, or obfuscates executables and file contents on the system or in transit to evade detection.
- **Detection:** Monitor for abnormal command-line syntax or **PowerShell obfuscation** patterns.
- **Mitigation:** Enable **Attack Surface Reduction (ASR)** rules on Windows 10+ endpoints to block execution of potentially obfuscated payloads.

### Behavior Profile 2 — T1059: Command and Scripting Interpreter

- **Tactic:** Execution
- **What the malware does:** Abuses command and scripting interpreters (e.g. `powershell.exe`, `cmd.exe`, `wscript.exe`) to execute commands, scripts, or binaries.
- **Detection:** Behavioural detection of scripting interpreter abuse outside expected administrative time windows or from abnormal user contexts.
- **Mitigation:** Deploy **Antivirus/Antimalware** to automatically quarantine suspicious files. Enforce **code signing** policies to permit execution of signed scripts only.

---

## Executive Summary & Findings

- **Threat Classification:** *Critical — Remote Access Trojan (RAT) & Loader*
- **Prepared For:** Cleveland Fintech Security Operations Center (SOC)

**Summary:**
The malware sample was successfully profiled and attributed to the **DarkGate** threat group. Once deployed on a host, the RAT establishes an unauthorized outbound connection to bypass the network perimeter and communicates with attacker-controlled infrastructure using the **Non-Standard Port** technique to avoid firewall detection.

**Recommended Actions:**

1. Immediately **block** the following IPs at the firewall:
   - `176.126.86.247`
   - `192.121.108.99`
2. **Update Intrusion Detection System (IDS)** rules to flag C2 beacon patterns matching this playbook.
3. **Quarantine** any host that has executed `setup.exe` and conduct a full forensic review.
4. Enable **ASR rules** across all Windows 10+ endpoints to reduce obfuscated payload execution risk.
5. Enforce **code signing** policies to limit scripting interpreter abuse.

---

## Tools and Technologies

- **VirusTotal** — malware hash analysis and antivirus detection scoring
- **MITRE ATT&CK Framework** — attacker behaviour mapping
- **Threat Intelligence methodology** — IOC extraction and infrastructure profiling
- **SHA256 / SHA1 / MD5** — digital fingerprinting and file integrity verification

---

## Author

**Obinna Mbah**
SOC Analyst — 10Alytics
📍 Gelsenkirchen, Germany
📅 Date of Investigation: 21 June 2026
