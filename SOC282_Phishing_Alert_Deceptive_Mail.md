# 🚨 SOC282 - Phishing Alert - Deceptive Mail Detected

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Verdict:** 🟢 True Positive (Phishing / User Execution & Malware Infection)  

---

## 📊 Executive Summary

| Field | Details |
| :--- | :--- |
| **Event ID** | 257 |
| **Severity** | Medium |
| **Category** | Exchange / Phishing |
| **Rule Name** | SOC282 - Phishing Alert - Deceptive Mail Detected |
| **Event Time** | 2024-05-13 09:22:00 +03:00 |
| **Sender Address** | `free@coffeeshooop.com` |
| **SMTP Server IP** | `103.80.134.63` |
| **Destination Address** | `Felix@letsdefend.io` |
| **Impacting Host / User** | Felix's Host |
| **Email Subject** | Free Coffee Voucher |
| **Malicious URL** | `https://files-ld.s3.us-east-2.amazonaws[.]com/free-coffee.zip` |
| **Payload Attachment** | `Free_coffee[.]zip` |
| **Device Action** | Allowed |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS IR Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** Email gateway flagged a deceptive inbound email with subject `Free Coffee Voucher` originating from a suspicious typosquatted domain (`coffeeshooop.com`).
* **Delivery Status:** The email bypassed initial perimeter controls and was successfully delivered (`Allowed`) to Felix's inbox.

### Phase 2: Identification & Analysis
* **Email & Payload Analysis:**
  * Inspection of the email body revealed a hyperlinked text pointing to an Amazon S3 bucket hosting a zipped archive: `https://files-ld.s3.us-east-2.amazonaws.com/free-coffee.zip`.
  * Threat Intelligence lookups (VirusTotal, ANY.RUN) confirmed that `Free_coffee.zip` contained a malicious executable payload disguised as a promotional voucher.
* **Host & Process Forensics:**
  * Log management and process tracking revealed that user **Felix** clicked the embedded hyperlink, downloaded `Free_coffee.zip`, extracted the archive, and executed the malicious binary on his workstation.

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Mailbox Purge:** Deleted the phishing email from Felix's inbox and purged it across the Exchange environment.
  * **Host Isolation:** Immediately isolated Felix's workstation from the network to prevent command-and-control (C2) communication and lateral movement.
  * **Perimeter Blocking:** Blocked the sender domain (`coffeeshooop.com`), SMTP IP (`103.80.134.63`), and malicious S3 URL across firewalls and web proxies.
* **Eradication:**
  * Terminated malicious processes associated with the execution of `Free_coffee.zip`.
  * Cleared dropped malware artifacts and persistence mechanisms from the host.
  * Initiated a password reset and revoked active session tokens for user Felix.
* **Recovery:**
  * Conducted full EDR endpoint scans to confirm complete malware remediation.
  * Reconnected the endpoint to the enterprise network once verified clean.

### Phase 4: Post-Incident Activity & Lessons Learned
* **Gateway Rules:** Update secure email gateway (SEG) rules to strictly block deceptive typosquatted domains targeting staff.
* **Security Awareness:** Conduct targeted user training on social engineering tactics leveraging fake rewards and vouchers.

---

## 🎯 MITRE ATT&CK Mapping

* **T1566 - Phishing:** Delivering deceptive messages via email to gain initial access.
* **T1566.002 - Spearphishing Link:** Including malicious URLs within email messages to direct targets to payload downloads.
* **T1059 - Command and Scripting Interpreter:** Executing scripts or commands following payload extraction.
* **T1204 - User Execution:** Relying on user interaction (clicking link, unzipping file, running binary) to execute malicious code.
