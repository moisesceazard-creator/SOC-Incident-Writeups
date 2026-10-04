# 🛡️ SOC Incident Investigation Write-Ups

A collection of **LetsDefend SOC training investigations** documenting alert triage, evidence analysis, IOC identification, threat intelligence validation, incident assessment, MITRE ATT&CK mapping, and recommended SOC response actions.

> **Training Context:** All cases in this repository were investigated in the **LetsDefend simulated SOC environment**. IP addresses, domains, hashes, email addresses, usernames, and hostnames are lab/training data and do not represent real production incidents.
>
> These write-ups demonstrate hands-on SOC investigation skills and **do not claim professional production SOC experience**.

---

## 👤 Analyst

**Moises Ceazar Del Mundo**

**Focus:** SOC Operations • Blue Team • Security Monitoring • Threat Detection • Incident Investigation

---

## 🎯 What This Repository Demonstrates

These investigations focus on the analytical workflow expected of a Tier 1 SOC analyst:

- **Alert Triage** — validating alerts and determining True Positive / False Positive findings
- **Evidence Correlation** — connecting email, endpoint, network, HTTP, and threat-intelligence evidence
- **IOC Analysis** — identifying suspicious IPs, domains, URLs, hashes, files, and processes
- **Phishing Investigation** — phishing, typosquatting, quishing, and ClickFix analysis
- **Malware Analysis** — PowerShell, malware payloads, information stealers, and execution chains
- **Web Attack Analysis** — exploitation of public-facing applications and security appliances
- **Vulnerability Investigation** — analyzing exploitation activity associated with CVEs
- **MITRE ATT&CK Mapping** — mapping observed or case-supported attacker techniques
- **Incident Scoping** — determining affected users, hosts, infrastructure, and potential impact
- **SOC Response Planning** — documenting appropriate containment, eradication, recovery, and prevention recommendations

---

## 🛠️ Tools & Technologies

| Area | Tools / Platforms |
|---|---|
| SOC Simulation | LetsDefend |
| Threat Intelligence | VirusTotal, AbuseIPDB, LetsDefend Threat Intelligence |
| Malware Analysis | ANY.RUN, VirusTotal |
| Network / Web Analysis | HTTP logs, network telemetry, WAF/IDS evidence |
| Framework | MITRE ATT&CK |
| Investigation Methodology | Structured SOC triage and incident investigation workflow |

---

## 📊 Case Index

| ID | Case | Severity | Category | Event ID | Status |
|:---:|---|:---:|---|:---:|:---:|
| **SOC274** | Palo Alto Networks PAN-OS Command Injection — CVE-2024-3400 | 🔴 Critical | Web Attack | 249 | ✅ Revised |
| **SOC257** | VPN Connection Detected from Unauthorized Country | 🟢 Low | Unauthorized Access | 225 | ✅ Revised |
| **SOC153** | Suspicious PowerShell Script Executed | 🟡 Medium | Malware | 238 | ✅ Revised |
| **SOC326** | Impersonating Domain MX Record Change Detected | 🟡 Medium | Threat Intelligence / Phishing | 304 | ✅ Revised |
| **SOC336** | Windows OLE RCE Exploitation — CVE-2025-21298 | 🔴 Critical | Malware / Exploit | 314 | ✅ Revised |
| **SOC338** | Lumma Stealer — DLL Side-Loading via ClickFix Phishing | 🔴 Critical | Malware / Data Leakage | 316 | ✅ Revised |
| **SOC335** | CVE-2024-49138 Exploitation Detected | 🟡 Medium | Privilege Escalation | 313 | ✅ Revised |
| **SOC342** | SharePoint ToolShell Auth Bypass & RCE — CVE-2025-53770 | 🔴 Critical | Web Attack | 320 | ✅ Revised |
| **SOC282** | Phishing Alert — Deceptive Mail Detected | 🟡 Medium | Phishing | 257 | ✅ Revised |
| **SOC287** | Arbitrary File Read on Check Point Gateway — CVE-2024-24919 | 🟠 High | Web Attack | 263 | ✅ Revised |
| **SOC251** | Quishing Detected — QR Code Phishing | 🟡 Medium | Phishing | 214 | ✅ Revised |

---

## 🔎 Investigation Themes

### 📧 Phishing & Social Engineering

Investigations include:

- Deceptive phishing emails
- Typosquatted domains
- QR-code phishing / **Quishing**
- ClickFix social engineering
- Malicious links and attachments
- User-execution analysis

Examples:

- **SOC282** — Phishing with malicious ZIP payload
- **SOC251** — QR-code phishing leading to a fake MFA page
- **SOC326** — Typosquatted domain and MX-record investigation
- **SOC338** — ClickFix phishing leading to Lumma Stealer

---

### 🦠 Malware & Endpoint Investigation

The repository also covers endpoint-focused investigation techniques including:

- PowerShell execution analysis
- Suspicious process investigation
- Process masquerading
- Malware payload analysis
- DLL side-loading
- Information-stealer investigation
- C2 communication correlation

Examples:

- **SOC153** — Suspicious PowerShell execution
- **SOC335** — `svohost.exe` masquerading and privilege-escalation activity
- **SOC338** — Lumma Stealer DLL side-loading
- **SOC336** — Malicious RTF exploitation and outbound C2

---

### 🌐 Web & Vulnerability Exploitation

Several investigations focus on attacks against public-facing applications and security infrastructure:

- Exploit detection
- HTTP request analysis
- Path traversal
- Remote code execution
- Authentication bypass
- CVE-based exploitation
- Exploitation success verification

Examples:

- **SOC274** — PAN-OS command injection
- **SOC287** — Check Point arbitrary file read
- **SOC342** — SharePoint ToolShell authentication bypass and RCE

---

## 📋 Investigation Methodology

Each write-up is structured around a consistent SOC investigation workflow while remaining grounded in the evidence available in the LetsDefend exercise.

### 1. Detection & Initial Triage

Identify:

- What triggered the alert
- Alert severity and category
- Affected host/user
- Source and destination information
- Initial suspicious indicators

### 2. Identification & Analysis

Correlate available evidence such as:

- IP addresses
- Domains and URLs
- File hashes
- Email artifacts
- HTTP requests
- Process information
- Endpoint telemetry
- Network activity
- Threat-intelligence results

### 3. Analyst Assessment

Determine:

- True Positive / False Positive
- Attack or execution chain
- Scope of observed activity
- Confirmed findings vs. potential impact
- Appropriate severity and escalation considerations

### 4. Recommended SOC Response

Where the exercise describes response actions, the portfolio distinguishes them from personally executed production actions.

Recommendations may include:

- Containment
- Blocking malicious infrastructure
- Host isolation
- Artifact removal
- Credential protection
- Vulnerability remediation
- Recovery validation

> **Important:** Recommended response actions are presented as analyst recommendations based on the training scenario. They are not represented as actions performed in a real production SOC.

### 5. Detection & Prevention Recommendations

Each case identifies practical defensive improvements where supported by the scenario, such as:

- Detection-rule tuning
- Email-security improvements
- Endpoint monitoring
- WAF/IDS monitoring
- Vulnerability management
- User awareness
- Process and network correlation

### 6. MITRE ATT&CK Mapping

Relevant techniques are mapped to MITRE ATT&CK.

Where the original LetsDefend case contains techniques that are not directly demonstrated by the available evidence, the write-up identifies them as **case-referenced mappings** rather than presenting them as independently confirmed findings.

---

## 🧠 Analyst Approach

The goal of these write-ups is not simply to document that an alert was triggered.

The investigation focuses on answering:

> **What happened, what evidence proves it, how far did the activity progress, and what should a SOC analyst do next?**

The investigations therefore emphasize:

**Alert → Evidence → Correlation → Assessment → Response Recommendation**

---

## 📈 Skills Demonstrated

Through these simulated investigations, the repository demonstrates practice in:

- SOC Tier 1 alert triage
- Security-event investigation
- Phishing analysis
- Malware triage
- Threat-intelligence validation
- IOC extraction
- Email-security analysis
- Network and HTTP log analysis
- Endpoint investigation
- PowerShell analysis
- Process-tree analysis
- CVE exploitation analysis
- Web-attack investigation
- Incident scoping
- MITRE ATT&CK mapping
- Recommended containment and remediation planning
- Security detection and prevention thinking

---

## ⚠️ Training Disclaimer

This repository represents **hands-on cybersecurity training through LetsDefend simulated SOC investigations**.

It should not be interpreted as professional production SOC experience.

All indicators and identities used in the cases are part of the training environment. Response actions described as recommendations are analytical recommendations based on the scenario and are not claims of actions performed against real production systems.

The purpose of this repository is to demonstrate the analyst's ability to:

**triage alerts → investigate evidence → correlate security events → identify threats → assess incidents → document findings → recommend appropriate SOC response.**
