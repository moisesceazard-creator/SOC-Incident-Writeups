# 🚨 SOC251 - Quishing Detected (QR Code Phishing)

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Verdict:** 🟢 True Positive (100% Playbook Score)  

---

## 📊 Executive Summary

| Field | Details |
| :--- | :--- |
| **Event ID** | 214 |
| **Severity** | Medium |
| **Category** | Phishing / Quishing |
| **Rule Name** | SOC251 - Quishing Detected (QR Code Phishing) |
| **Event Time** | 2024-01-01T12:37:38+03:00 |
| **Target User / Host** | `Claire@letsdefend.io` (`172.16.17.72`) |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** Security gateway rule flagged an incoming phishing email titled *"New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)"*.
* **Initial Observation:** Message delivered to `Claire@letsdefend.io` from sender `security@microsecmfa.com` via external IP `158.69.201.47`. Device action recorded as `Allowed`.

### Phase 2: Identification & Analysis
* **Artifact & Indicator Triage (IoCs):**
  * **Sender Address:** `security@microsecmfa.com`
  * **Recipient Address:** `Claire@letsdefend.io`
  * **Target Host IP:** `172.16.17.72`
  * **Source IP Address:** `158.69.201.47` (External)
  * **Subject:** `New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)`
  * **Phishing Vector:** Embedded QR Code (Quishing)
* **Domain & Reputation Analysis:** Sender domain `microsecmfa.com` identified as a typosquatting attempt spoofing Microsoft MFA services. Threat Intelligence platforms flagged source IP `158.69.201.47` for hosting active credential-harvesting infrastructure.
* **Payload Analysis:** Decoded the embedded QR code image from the email body; confirmed it redirected to an external landing page imitating an MFA login prompt.
* **Scope Determination:** Log Management inspection confirmed communication was limited exclusively to `172.16.17.72` with no broader internal spread.

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Perimeter Blocking:** Blacklisted source IP `158.69.201.47` and domain `microsecmfa.com` at the firewall and email gateway levels.
  * **Account Protection:** Revoked active sessions and initiated an emergency password reset for `Claire@letsdefend.io` to neutralize potential credential compromise.
* **Eradication:**
  * **Mail Purge:** Executed a global search-and-purge to remove the phishing message across all organizational mailboxes.
* **Recovery:**
  * **Identity Restoration:** Verified post-reset account activity and safely restored user access following security verification.

### Phase 4: Post-Incident Activity & Lessons Learned
* **QR Code / OCR Inspection Gap:** The email bypassed initial filters (`Allowed`) because QR codes render as image objects rather than plain text links. Enable Optical Character Recognition (OCR) and QR-code extraction features on the Secure Email Gateway (SEG).
* **Lookalike Domain Filtering:** Enforce stricter domain-reputation rules and typosquatting detection for incoming emails referencing security updates or authentication providers.
* **User Awareness Training:** Launch targeted campaigns educating employees on "Quishing" tactics, highlighting the dangers of scanning unexpected QR codes using personal or corporate mobile devices.

---

## 🎯 MITRE ATT&CK Mapping

* **T1589.002 - Gather Victim Identity Information:** Email Addresses
* **T1598.003 - Phishing for Information:** Spearphishing Link (Quishing via QR Code)
* **T1036 - Masquerading:** Domain spoofing targeting MFA security update themes
* **T1140 - Deobfuscate/Decode Files or Information:** Extraction and decoding of embedded QR payloads
