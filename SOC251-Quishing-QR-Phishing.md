## 🔍 SOC Incident Write-Up: SOC251 - Quishing Detected (QR Code Phishing)

**Platform:** LetsDefend
**Case Name:** SOC251 - Quishing Detected (QR Code Phishing)
**Event ID:** 214 | **Severity:** Medium | **Difficulty:** Easy
**Analyst:** Moises Ceazar Del Mundo
**Target Email:** Claire@letsdefend.io (172.16.17.72)

> Note: This case is based on a simulated SOC alert (LetsDefend lab environment). IOCs, IPs, domains, and user identities are lab/training data.

---

### 1. Executive Summary

A phishing alert triggered regarding a suspicious email titled *"New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)."* Investigation confirmed the email originated from an untrusted external domain (`microsecmfa.com`) and contained a malicious QR code (Quishing) designed to harvest user credentials. The alert was classified as a **True Positive**, and containment measures were initiated to safeguard compromised credentials.

---

### 2. Alert Details & Telemetry Data

| Metric | Details |
|---|---|
| Rule Name | SOC251 - Quishing Detected (QR Code Phishing) |
| Event Time | 2024-01-01T12:37:38+03:00 |
| Closed At | 2026-02-19T03:15:28+00:00 |
| SLA | 1555.98 |
| Sender Address | security@microsecmfa.com |
| Recipient Address | Claire@letsdefend.io |
| Source IP | 158.69.201.47 (External) |
| Destination Host IP | 172.16.17.72 |
| Email Subject | New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA) |
| Device Action | Allowed |
| Playbook Score | 25 (100% success rate) |
| Result | True Positive |

---

### 3. Incident Investigation & Triage Workflow

**Step 1: Email Header & Sender Verification**
- Analyzed the sender domain `microsecmfa.com`. Identified typosquatting/masquerading attempt spoofing legitimate Microsoft MFA communications.

**Step 2: Reconnaissance Identification**
- Determined the type of reconnaissance used by the attacker: **Phishing for Information** — the attacker sent a Quishing email to the target user to harvest credentials (MITRE T1589.002).

**Step 3: Threat Intelligence & IP Reputation Analysis**
- Queried external Threat Intelligence platforms using source IP `158.69.201.47`.
- Result: IP flagged as suspicious and reported multiple times by security researchers for hosting malicious phishing infrastructure.

**Step 4: Quishing & Artifact Analysis**
- Inspected email payload containing a QR code embedded in the body.
- Decoded QR link pointing to an external credential-harvesting landing page mimicking an MFA login prompt.

**Step 5: Scope Determination**
- Checked Log Management for the attacker IP (`158.69.201.47`). Only `172.16.17.72` observed as the destination IP — confirming the email was delivered to Claire@letsdefend.io with no lateral spread to other internal hosts.

**Step 6: Containment Decision**
- Since the user's credentials and mailbox were potentially compromised, the containment process was initiated.

---

### 4. Indicators of Compromise (IOCs)

- **Sender Address:** security@microsecmfa.com
- **Attacker IP:** 158.69.201.47
- **Target User:** Claire@letsdefend.io
- **Target IP:** 172.16.17.72

---

### 5. MITRE ATT&CK Mapping

| Tactic | Technique ID | Technique Name |
|---|---|---|
| Reconnaissance (TA0043) | T1589.002 | Gather Victim Identity Information: Email Addresses |
| Reconnaissance (TA0043) | T1598.002 | Phishing for Information: Spearphishing Attachment |
| Reconnaissance (TA0043) | T1598.003 | Phishing for Information: Spearphishing Link (Quishing) |
| Resource Development / Defense Evasion (TA0042/TA0005) | T1036 | Masquerading: Domain spoofing targeting security updates |
| Credential Access (TA0006) | T1140 | Deobfuscate/Decode Files or Information: QR code payload extraction |

---

### 6. Playbook Verdict & Containment Actions

**Final Verdict:** True Positive

**Remediation Actions Taken:**
1. **Mail Quarantine:** Purged malicious phishing email from the recipient mailbox (Claire@letsdefend.io).
2. **IP & Domain Blocking:** Blocked IP `158.69.201.47` and domain `microsecmfa.com` at the email gateway and firewall level.
3. **Credential Reset:** Initiated password reset and session revocation for Claire@letsdefend.io.
4. **Security Awareness:** Recommended pushing a user notification alert regarding QR-code-based phishing tactics (Quishing).

---

### 7. Lessons Learned / Recommendations

- Email gateway allowed the message through (Device Action: *Allowed*) despite the sender domain being a close typosquat of a legitimate Microsoft-related domain — recommend tightening lookalike-domain detection rules for MFA/security-themed subject lines.
- QR-code payloads bypass traditional URL/link-scanning controls since the malicious link is embedded as an image rather than plain text; recommend enabling QR-code/image decoding in the email security gateway.
- Conduct targeted user awareness training on Quishing, given it is a growing phishing vector that evades conventional text-based filters.
