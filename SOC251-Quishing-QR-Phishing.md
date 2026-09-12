# 🔍 SOC Incident Write-Up: SOC251 - Quishing Detected (QR Code Phishing)

**Platform:** LetsDefend  
**Case Name:** SOC251 - Quishing Detected (QR Code Phishing)  
**Event ID:** 214 | **Severity:** Medium | **Difficulty:** Easy  
**Analyst:** Moises Ceazar Del Mundo, CC  
**Target Email:** Claire@letsdefend.io (`172.16.17.72`)  

---

## 1. Executive Summary

A phishing alert triggered regarding a suspicious email titled **"New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)"**. Investigation confirmed that the email originated from a untrusted external domain (`microsecmfa.com`) and contained a malicious QR Code (**Quishing**) designed to harvest user credentials. The alert was classified as a **True Positive**, and containment measures were initiated to safeguard compromised credentials.

---

## 2. Alert Details & Telemetry Data

| Metric | Details |
| :--- | :--- |
| **Rule Name** | `SOC251 - Quishing Detected (QR Code Phishing)` |
| **Event Time** | `2024-01-01 12:37:38 UTC+3` |
| **Sender Address** | `security@microsecmfa.com` |
| **Recipient Address** | `Claire@letsdefend.io` |
| **Source IP** | `158.69.201.47` (External) |
| **Destination Host IP** | `172.16.17.72` |
| **Email Subject** | `New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)` |

---

## 3. Incident Investigation & Triage Workflow

### Step 1: Email Header & Sender Verification
* Analyzed the sender domain `microsecmfa.com`. Identified typosquatting/masquerading attempt spoofing legitimate Microsoft MFA communications.

### Step 2: Threat Intelligence & IP Reputation Analysis
* Queried external Threat Intelligence platforms using source IP `158.69.201.47`. 
* **Result:** IP flagged across multiple security vendors for hosting malicious phishing infrastructure.

### Step 3: Quishing & Artifact Analysis
* Inspected email payload containing a QR code embedded in the body.
* Decoded QR link pointing to an external credential harvesting landing page mimicking an MFA login prompt.

### Step 4: Scope Determination
* Checked Exchange & Log Management logs for `158.69.201.47`. Confirmed the email was delivered to `172.16.17.72` (`Claire@letsdefend.io`) with no further lateral spreading observed across other internal hosts.

---

## 4. Indicators of Compromise (IOCs)

* **Sender Address:** `security@microsecmfa.com`
* **Attacker IP:** `158.69.201.47`
* **Target User:** `Claire@letsdefend.io`
* **Target IP:** `172.16.17.72`

---

## 5. MITRE ATT&CK Mapping

* **T1589.002 - Gather Victim Identity Information:** Email Addresses
* **T1598.002 - Phishing for Information: Spearphishing Attachment / Link (Quishing)**
* **T1036 - Masquerading:** Domain spoofing targeting security updates
* **T1140 - Deobfuscate/Decode Files or Information:** QR Code payload extraction

---

## 6. Playbook Verdict & Containment Actions

* **Final Verdict:** **True Positive**
* **Remediation Actions Taken:**
  1. **Mail Quarantine:** Purged malicious phishing email from the recipient mailbox (`Claire@letsdefend.io`).
  2. **IP & Domain Blocking:** Blocked IP `158.69.201.47` and domain `microsecmfa.com` on email gateway and firewall level.
  3. **Credential Reset:** Initiated password reset and session revocation for `Claire@letsdefend.io`.
  4. **Security Awareness:** Recommended pushing a user notification alert regarding QR-code-based phishing tactics (Quishing).
