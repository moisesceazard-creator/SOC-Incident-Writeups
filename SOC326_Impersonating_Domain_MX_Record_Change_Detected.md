# 🚨 SOC326 - Impersonating Domain MX Record Change Detected

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Verdict:** 🟢 True Positive (Phishing / Typosquatted Domain Access)  

---

## 📊 Executive Summary

| Field | Details |
| :--- | :--- |
| **Event ID** | 304 |
| **Severity** | Medium |
| **Category** | ThreatIntel / Phishing |
| **Rule Name** | SOC326 - Impersonating Domain MX Record Change Detected |
| **Event Time** | 2024-09-17 12:05:00 +03:00 |
| **Source Address** | `no-reply@cti-report.io` |
| **Destination Address** | `soc@letsdefend.io` |
| **Impacting Host / User** | Mateo's Host |
| **Typosquatted Domain** | `letsdwfend[.]io` |
| **New MX Record** | `mail.mailerhost[.]net` |
| **Device Action** | Allowed |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS IR Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** Threat intelligence monitoring flagged a newly configured MX record (`mail.mailerhost.net`) on a typosquatted domain (`letsdwfend.io`) mimicking the legitimate `letsdefend.io` domain.
* **Notification Email:** A notification email was received from `no-reply@cti-report.io` to `soc@letsdefend.io` containing the hyperlink to the typosquatted domain.

### Phase 2: Identification & Analysis
* **Email & URL Analysis:**
  * Inspection of the delivered message confirmed an embedded hyperlink directing to `letsdwfend.io`.
  * Threat Intel lookups (AbuseIPDB, VirusTotal, and LetsDefend Threat Intel) flagged `letsdwfend.io` as a malicious typosquatting domain setup for brand impersonation and credential harvesting.
* **Mail Delivery & Execution Verification:**
  * Exchange logs showed the email was successfully delivered to the mailbox (`Allowed`).
  * Proxy and Network logs showed that user **Mateo** clicked the malicious link and his host successfully established a connection to `letsdwfend.io`.

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Mailbox Purge:** Purged and deleted the phishing email directly from the recipient's mailbox.
  * **Host Isolation:** Isolated Mateo's host from the local network to contain potential post-exploitation or credential compromise.
  * **Domain & IP Block:** Blocked `letsdwfend.io` and associated infrastructure across perimeter firewalls, DNS filters, and secure web gateways.
* **Eradication:**
  * Initiated password reset procedures and revoked active session tokens for user Mateo.
  * Performed an EDR scan on Mateo's host to verify no secondary payloads were dropped following the web access.
* **Recovery:**
  * Verified host cleanliness and restored network access after resetting user credentials.

### Phase 4: Post-Incident Activity & Lessons Learned
* **DNS / Brand Monitoring:** Strengthen automated typosquatting detection to proactively sinkhole lookalike domains prior to user exposure.
* **Security Awareness Training:** Provide targeted phishing training regarding typosquatted domain links (e.g., subtle spelling changes like `letsdwfend` vs `letsdefend`).

---

## 🎯 MITRE ATT&CK Mapping

* **T1598.003 - Phishing for Information: Spearphishing Link:** Adversaries sending emails with malicious links targeting specific users or groups.
* **T1566 - Phishing:** Delivering malicious links via email messages to gain initial access or credentials.
* **T1656 - Impersonation:** Registering domains and infrastructure mimicking legitimate enterprise domains to deceive targets.
