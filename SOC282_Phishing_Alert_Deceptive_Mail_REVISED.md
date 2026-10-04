# SOC282 — Phishing Alert: Deceptive Mail Detected

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Environment:** Simulated SOC investigation  
**Verdict:** True Positive — Phishing / User Execution & Malware Infection

> **Training Context:** This investigation was performed in the LetsDefend training environment. The email addresses, domains, IP addresses, hosts, and other indicators documented here are lab/training data and do not represent a real production incident.

---

## 1. Incident Overview

| Field | Details |
|---|---|
| Event ID | 257 |
| Severity | Medium |
| Category | Exchange / Phishing |
| Detection Rule | SOC282 — Phishing Alert - Deceptive Mail Detected |
| Event Time | 2024-05-13 09:22:00 +03:00 |
| Sender | `free@coffeeshooop.com` |
| SMTP Server IP | `103.80.134.63` |
| Recipient | `Felix@letsdefend.io` |
| Impacted Host/User | Felix's Host |
| Subject | `Free Coffee Voucher` |
| Malicious URL | `https://files-ld.s3.us-east-2.amazonaws[.]com/free-coffee.zip` |
| Payload | `Free_coffee[.]zip` |
| Device Action | Allowed |
| Final Assessment | True Positive |

---

## 2. Detection & Initial Triage

The alert identified a deceptive inbound email using the subject **“Free Coffee Voucher.”** The sender domain, `coffeeshooop.com`, was identified as suspicious and consistent with a typosquatting-based phishing attempt.

The email was delivered to Felix's mailbox because the device action was recorded as **Allowed**.

### Initial Indicators

- Suspicious sender domain: `coffeeshooop.com`
- Sender IP: `103.80.134.63`
- Recipient: `Felix@letsdefend.io`
- Malicious archive: `Free_coffee[.]zip`
- External download URL hosted on Amazon S3
- Promotional lure: free coffee voucher

The combination of a deceptive sender domain, a promotional lure, and a ZIP archive provided sufficient indicators for deeper investigation.

---

## 3. Email & Payload Analysis

### Phishing Email

The email used a promotional offer to encourage the recipient to interact with the message.

The sender domain `coffeeshooop.com` was identified as a suspicious typosquatted domain, increasing the likelihood that the message was designed to impersonate a legitimate organization or service.

The message contained a hyperlink pointing to:

```text
https://files-ld.s3.us-east-2.amazonaws[.]com/free-coffee.zip
```

The destination hosted a ZIP archive named:

```text
Free_coffee[.]zip
```

### Malware Analysis

VirusTotal and ANY.RUN analysis confirmed that the archive contained a **malicious executable disguised as a promotional voucher**.

This established that the email was not only a phishing attempt but also delivered a malicious payload capable of execution on the endpoint.

---

## 4. User Interaction & Attack Chain

Log and process tracking provided evidence of the following sequence:

```text
Phishing Email
      ↓
User Clicked Link
      ↓
ZIP Archive Downloaded
      ↓
Archive Extracted
      ↓
Malicious Binary Executed
      ↓
Potential Endpoint Compromise
```

The investigation therefore progressed beyond simple phishing detection.

The available evidence shows that the user **clicked the malicious link, downloaded the archive, extracted it, and executed the malicious binary**.

This user interaction was the key factor supporting the final True Positive assessment.

---

## 5. Analyst Assessment

**Final Verdict: TRUE POSITIVE**

This alert represents a successful phishing-based malware delivery attempt.

The investigation correlated multiple sources of evidence:

1. The sender used a suspicious typosquatted domain.
2. The email contained a link to an externally hosted ZIP archive.
3. VirusTotal and ANY.RUN identified the archive as malicious.
4. Logs showed that Felix clicked the link.
5. The archive was downloaded and extracted.
6. Process tracking showed execution of the malicious binary.

The evidence supports classification as **Phishing / User Execution & Malware Infection**, rather than a benign or blocked phishing attempt.

---

## 6. Recommended SOC Response

The original LetsDefend scenario describes response actions such as mailbox cleanup, host isolation, blocking malicious infrastructure, terminating malicious processes, and credential/session remediation.

For portfolio accuracy, these should be treated as **recommended SOC response actions**, not as production actions personally performed by the analyst.

### Recommended Containment

- Isolate Felix's endpoint from the network if compromise is suspected.
- Block the identified malicious URL and associated sender/domain through available security controls.
- Remove or quarantine the phishing email from affected mailboxes.
- Prevent further execution of the identified malicious payload.

### Recommended Eradication & Recovery

- Terminate malicious processes associated with the downloaded payload.
- Remove the malicious archive and executable.
- Perform an endpoint security scan and investigate potential persistence.
- Review authentication activity for signs of credential compromise.
- Reset credentials and revoke sessions if compromise is suspected.
- Restore normal endpoint connectivity only after the system has been validated.

### Recommended Follow-Up

- Review mail security controls for similar typosquatted domains.
- Search for additional recipients of the same phishing campaign.
- Add confirmed malicious indicators to appropriate detection/blocking controls.
- Use the incident as a security-awareness example for users.

---

## 7. Detection & Prevention Recommendations

### Email Security

- Improve detection of typosquatted sender domains.
- Inspect URLs contained in inbound messages before delivery.
- Detect archive attachments containing executable content.
- Apply stricter controls to suspicious external file-hosting services when appropriate.

### Endpoint Security

- Monitor execution of binaries extracted from user-downloaded archives.
- Alert on suspicious child processes originating from downloaded files.
- Correlate email activity with endpoint process execution.

### User Awareness

The incident demonstrates the risk of promotional phishing lures.

Users should be encouraged to verify unexpected offers and avoid opening downloaded archives from unsolicited emails, even when the message appears harmless or attractive.

---

## 8. MITRE ATT&CK Mapping

| Technique | ID | Relevance |
|---|---|---|
| Phishing | T1566 | The attack began with a deceptive email. |
| Spearphishing Link | T1566.002 | The email contained a link leading to the malicious archive. |
| User Execution: Malicious File | T1204.002 | The user downloaded, extracted, and executed the malicious binary. |

**Note:** The original case also references **T1059 — Command and Scripting Interpreter**. The available evidence in this write-up directly establishes execution of a malicious binary, but does not provide enough detail to independently confirm a command/scripting interpreter was used. Therefore, it is not treated as a primary mapping here.

---

## 9. Analyst Takeaway

This case demonstrates why a SOC analyst should investigate beyond the initial phishing alert.

The important finding was not simply that a suspicious email was delivered. The investigation established a complete user-driven attack chain:

**phishing email → malicious link → payload download → archive extraction → malware execution.**

For a Tier 1 SOC analyst, the key skills demonstrated are **alert triage, phishing analysis, IOC identification, threat-intelligence validation, log correlation, user-activity analysis, malware triage, and accurate incident classification.**

---

## 10. Skills Demonstrated

- SOC alert triage
- Phishing email analysis
- Typosquatting identification
- IOC extraction
- VirusTotal analysis
- ANY.RUN malware analysis
- Log and process correlation
- User-execution analysis
- Malware delivery investigation
- MITRE ATT&CK mapping
- Incident classification
- Recommended containment and remediation planning

---

## Training Disclaimer

This case study documents a **LetsDefend simulated SOC investigation**. It should be interpreted as hands-on cybersecurity training rather than professional production SOC experience.

The response recommendations are presented as analyst recommendations based on the evidence available in the exercise. No claim is made that these actions were executed against a real production environment.
