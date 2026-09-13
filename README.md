# SOC-Incident-Writeups

Documentation of hands-on SOC incident investigations, threat triage, MITRE ATT&CK mapping, and alert resolution playbooks.

> **Note:** Cases in this repo are based on simulated SOC alerts (LetsDefend-style lab environment). IOCs, IPs, domains, and user identities are lab/training data and do not reflect real production incidents.

## Analyst

Moises Ceazar Del Mundo

## Tools & Skills Demonstrated

- SIEM alert triage & investigation (LetsDefend platform)
- Email header & sender analysis (phishing, spoofing, quishing)
- Threat Intelligence / IP & domain reputation lookups
- Malware & PowerShell script analysis
- Log correlation (Exchange, firewall, EDR)
- MITRE ATT&CK technique mapping
- Incident containment & remediation playbooks

## Case Index

| Rule ID | Case Name | Severity | Type | Event ID | Date Closed | Write-Up |
|---------|-----------|----------|------|----------|-------------|----------|
| SOC274 | Palo Alto Networks PAN-OS Command Injection Vulnerability Exploitation (CVE-2024-3400) | Critical | Web Attack | 249 | 2026-07-09 | [SOC274-PANOS-CommandInjection-CVE-2024-3400.md](./SOC274-PANOS-CommandInjection-CVE-2024-3400.md) |
| SOC257 | VPN Connection Detected from Unauthorized Country | Low | Unauthorized Access | 225 | 2026-07-08 | Pending |
| SOC153 | Suspicious Powershell Script Executed | Medium | Malware | 238 | 2026-02-25 | Pending |
| SOC326 | Impersonating Domain MX Record Change Detected | Medium | ThreatIntel | 304 | 2026-02-23 | Pending |
| SOC336 | Windows OLE Zero-Click RCE Exploitation Detected (CVE-2025-21298) | Critical | Malware | 314 | 2026-02-22 | Pending |
| SOC338 | Lumma Stealer - DLL Side-Loading via ClickFix Phishing | Critical | Data Leakage | 316 | 2026-02-21 | Pending |
| SOC335 | CVE-2024-49138 Exploitation Detected | Medium | Privilege Escalation | 313 | 2026-02-21 | Pending |
| SOC342 | CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE | Critical | Web Attack | 320 | 2026-02-21 | Pending |
| SOC282 | Phishing Alert - Deceptive Mail Detected | Medium | Exchange | 257 | 2026-02-20 | Pending |
| SOC287 | Arbitrary File Read on Checkpoint Security Gateway (CVE-2024-24919) | High | Web Attack | 263 | 2026-02-19 | Pending |
| SOC251 | Quishing Detected (QR Code Phishing) | Medium | Exchange | 214 | 2026-02-19 | [SOC251-Quishing-QR-Phishing.md](./SOC251-Quishing-QR-Phishing.md) |

## Write-Up Structure

Each incident write-up follows a consistent format:

1. Executive Summary
2. Alert Details & Telemetry Data
3. Incident Investigation & Triage Workflow
4. Indicators of Compromise (IOCs)
5. MITRE ATT&CK Mapping
6. Playbook Verdict & Containment Actions
7. Lessons Learned / Recommendations
