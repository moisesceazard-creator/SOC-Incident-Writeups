# 🚨 SOC287 - Arbitrary File Read on Checkpoint Security Gateway (CVE-2024-24919)

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Verdict:** 🟢 True Positive (Successful Exploitation of Zero-Day / Critical Path Traversal Vulnerability)  

---

## 📊 Executive Summary

| Field | Details |
| :--- | :--- |
| **Event ID** | 263 |
| **Severity** | High |
| **Category** | Web Attack |
| **Rule Name** | SOC287 - Arbitrary File Read on Checkpoint Security Gateway [CVE-2024-24919] |
| **Event Time** | 2024-06-06 15:12:45 +03:00 |
| **Target Host** | `CP-Spark-Gateway-01` (`172.16.20.146`) |
| **Source IP Address** | `203.160.68.12` (External Internet) |
| **Target Endpoint** | `POST /clients/MyCRL` |
| **Exploit Payload** | `aCSHELL/../../../../../../../etc/passwd` |
| **User-Agent** | `Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:126.0) Gecko/20100101 Firefox/126.0` |
| **Device Action** | Allowed |
| **HTTP Response Code** | `200 OK` (Exploit Successful) |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS IR Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** The Web Application Firewall (WAF) / IDS flagged suspicious POST request characteristics matching path traversal patterns against Check Point Security Gateway appliances (`CVE-2024-24919`).
* **Initial Observation:** Incoming request originated from external IP `203.160.68.12` targeting internal gateway `CP-Spark-Gateway-01` (`172.16.20.146`) on endpoint `/clients/MyCRL`.

### Phase 2: Identification & Analysis
* **Traffic & Payload Analysis:**
  * Examination of HTTP traffic revealed a crafted POST request body containing path traversal sequences: `aCSHELL/../../../../../../../etc/passwd`.
  * The exploit target (`/clients/MyCRL`) abuses an unauthenticated path traversal vulnerability in Check Point VPN / Network Security Gateways to access system files.
* **Success Verification:**
  * Server logs confirmed the HTTP response status code was `200 OK`.
  * The gateway processed the path traversal request and returned system configuration/user account details (`/etc/passwd`), confirming successful unauthenticated arbitrary file read.
* **Scope & Impact Assessment:**
  * Sensitive password hashes and system configurations were exposed.
  * Risk of subsequent credential dumping, local account cracking, and lateral movement.

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Network Isolation:** Immediately isolated `CP-Spark-Gateway-01` (`172.16.20.146`) to prevent further unauthorized file access or exploitation attempts.
  * **IP Block:** Blacklisted malicious external IP `203.160.68.12` across perimeter firewalls and edge routers.
* **Eradication:**
  * **Patching & Remediation:** Applied official Check Point Security Gateway hotfixes addressing CVE-2024-24919.
  * **Credential Rotation:** Rotated all local gateway account passwords, service accounts, and VPN portal secrets due to exposure of `/etc/passwd` and potential shadow file access.
* **Recovery:**
  * Performed complete integrity checks on `CP-Spark-Gateway-01` to ensure no secondary web shells or backdoors were dropped.
  * Restored the gateway to production following patch verification and credential resetting.

### Phase 4: Post-Incident Activity & Escalation
* **Tier 2 Escalation:** Escalated case to Tier 2 / Incident Response Team due to confirmed zero-day/critical vulnerability exploitation and potential system credential disclosure.
* **Preventive Measures:**
  * Enforce strict WAF rules blocking directory traversal patterns (`../` sequences) on all public-facing security appliances.
  * Ensure rapid patch management processes for internet-exposed perimeter gateways and VPN devices.

---

## 🎯 MITRE ATT&CK Mapping

* **T1190 - Exploit Public-Facing Application:** Exploiting CVE-2024-24919 path traversal on Check Point Gateway to access internal system files.
* **T1003 - OS Credential Dumping:** Exfiltrating `/etc/passwd` and system configuration files to harvest user credentials.
* **T1083 - File and Directory Discovery:** Utilizing path traversal patterns to navigate directory structures and locate sensitive system files.
