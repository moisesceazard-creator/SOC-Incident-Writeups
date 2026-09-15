# 🛡️ SOC Incident Write-Ups & Threat Analysis

Documentation of hands-on SOC incident investigations, threat triage, MITRE ATT&CK mapping, and enterprise incident response playbooks following the NIST SP 800-61 / SANS IR framework.

> **Note:** Cases in this repo are based on simulated SOC alerts (LetsDefend lab environment). IOCs, IPs, domains, and user identities are lab/training data and do not reflect real production incidents.

---

## 👤 Security Analyst

**Moises Ceazar Del Mundo, CC**  

---

## 🛠️ Tools & Skills Demonstrated

- **SIEM Triage & Triage Workflows:** Alert investigation & playbook execution (LetsDefend platform).
- **Phishing & Email Security:** Header analysis, domain reputation, typosquatting, and Quishing (QR code) analysis.
- **Threat Intelligence:** IoC extraction, VirusTotal / IP reputation lookups, and C2 tracking.
- **Malware & Endpoint Analysis:** Command-line forensics, PowerShell script execution analysis, and zero-click vector tracking.
- **Incident Response Lifecycle:** End-to-end containment, eradication, recovery, and post-incident root-cause analysis.
- **MITRE ATT&CK Mapping:** Aligning adversary tactics and techniques with defense controls.

---

## 📊 Case Index

| Rule ID | Case Name | Severity | Type | Event ID | Date Closed | Write-Up Status |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| **SOC274** | Palo Alto Networks PAN-OS Command Injection (CVE-2024-3400) | 🔴 Critical | Web Attack | 249 | 2026-07-09 | [Read Write-Up](./SOC274_PAN_OS_Command_Injection.md) |
| **SOC257** | VPN Connection Detected from Unauthorized Country | 🟢 Low | Unauthorized Access | 225 | 2026-07-08 | [Read Write-Up](./SOC257_VPN_Connection_Unauthorized_Country.md) |
| **SOC153** | Suspicious Powershell Script Executed | 🟡 Medium | Malware | 238 | 2026-02-25 | *Pending* |
| **SOC326** | Impersonating Domain MX Record Change Detected | 🟡 Medium | ThreatIntel | 304 | 2026-02-23 | *Pending* |
| **SOC336** | Windows OLE Zero-Click RCE Exploitation (CVE-2025-21298) | 🔴 Critical | Malware | 314 | 2026-02-22 | [Read Write-Up](./SOC336_Windows_OLE_Zero_Click_RCE.md) |
| **SOC338** | Lumma Stealer - DLL Side-Loading via ClickFix Phishing | 🔴 Critical | Data Leakage | 316 | 2026-02-21 | *Pending* |
| **SOC335** | CVE-2024-49138 Exploitation Detected | 🟡 Medium | Privilege Escalation | 313 | 2026-02-21 | *Pending* |
| **SOC342** | CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE | 🔴 Critical | Web Attack | 320 | 2026-02-21 | [Read Write-Up](./SOC342_SharePoint_ToolShell_Auth_Bypass_RCE.md) |
| **SOC282** | Phishing Alert - Deceptive Mail Detected | 🟡 Medium | Exchange | 257 | 2026-02-20 | *Pending* |
| **SOC287** | Arbitrary File Read on Checkpoint Security Gateway (CVE-2024-24919) | 🟠 High | Web Attack | 263 | 2026-02-19 | *Pending* |
| **SOC251** | Quishing Detected (QR Code Phishing) | 🟡 Medium | Exchange | 214 | 2026-02-19 | [Read Write-Up](./SOC251_Quishing_Detected.md) |

---

## 📑 Write-Up Structure (NIST/SANS IR Framework)

Each incident write-up follows a standardized enterprise Incident Response lifecycle:

1. **Executive Summary & Metadata:** Metrics summary including Event ID, Severity, Target Host/User, and Final Verdict.
2. **Incident Response Phases:**
   * **Phase 1: Preparation & Detection:** Initial alert detection rules, incoming payload details, and initial security posture.
   * **Phase 2: Identification & Analysis:** Triage workflow, IoC extraction (IPs, hashes, domains), malware/payload analysis, and scope verification.
   * **Phase 3: Containment, Eradication & Recovery:** Host isolation, perimeter blocklists, artifact purging, and patch/recovery procedures.
   * **Phase 4: Post-Incident Activity & Lessons Learned:** Root-cause analysis, security posture gaps, and preventive recommendations.
3. **MITRE ATT&CK Mapping:** Direct alignment with adversary Tactics, Techniques, and Procedures (TTPs).
