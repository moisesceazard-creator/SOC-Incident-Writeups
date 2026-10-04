# 🚨 SOC326 — Impersonating Domain MX Record Change Detected

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Environment:** Simulated SOC training environment  
**Verdict:** 🟢 True Positive — Phishing / Typosquatted Domain Activity  

> **Training Context:** This investigation is based on a simulated SOC alert from the LetsDefend lab environment. The domains, IP addresses, email addresses, host information, and investigation details are training data and do not represent a production incident handled by the analyst.

---

## 📊 1. Incident Overview

| Field | Details |
| :--- | :--- |
| **Event ID** | 304 |
| **Severity** | Medium |
| **Category** | Threat Intelligence / Phishing |
| **Rule Name** | SOC326 - Impersonating Domain MX Record Change Detected |
| **Event Time** | 2024-09-17 12:05:00 +03:00 |
| **Source Address** | `no-reply@cti-report.io` |
| **Destination Address** | `soc@letsdefend.io` |
| **Impacting Host / User** | Mateo's Host |
| **Typosquatted Domain** | `letsdwfend[.]io` |
| **New MX Record** | `mail.mailerhost[.]net` |
| **Device Action** | Allowed |
| **Verdict** | True Positive |

---

## 🔎 2. Detection & Initial Triage

### Alert Trigger

Threat intelligence monitoring identified a newly configured MX record associated with the typosquatted domain:

```text
letsdwfend[.]io
```

The domain closely resembles the legitimate:

```text
letsdefend.io
```

The alert was accompanied by a notification email from:

```text
no-reply@cti-report.io
```

to:

```text
soc@letsdefend.io
```

The email contained a hyperlink directing users toward the suspicious domain.

### Initial Observations

- The domain used a spelling variation of the legitimate brand.
- A new MX record pointed to `mail.mailerhost[.]net`.
- The associated email was delivered successfully.
- The activity had potential phishing and brand-impersonation implications.

These indicators warranted further investigation into the domain, email, and subsequent user activity.

---

## 🧩 3. Email, Domain & Threat Intelligence Analysis

### Indicators Identified

| Indicator | Type | Observation |
| :--- | :--- | :--- |
| `letsdwfend[.]io` | Typosquatted Domain | Mimics the legitimate `letsdefend.io` domain |
| `mail.mailerhost[.]net` | MX Record | Newly configured mail infrastructure |
| `no-reply@cti-report.io` | Sender | Source of the notification email |
| `soc@letsdefend.io` | Recipient | Target SOC mailbox |
| Mateo's Host | Endpoint | User endpoint associated with subsequent activity |

### Typosquatting Analysis

The suspicious domain:

```text
letsdwfend.io
```

uses a subtle spelling modification of:

```text
letsdefend.io
```

This type of lookalike-domain construction can be used to make malicious infrastructure appear legitimate to users.

### Threat Intelligence Analysis

The original investigation used:

- AbuseIPDB
- VirusTotal
- LetsDefend Threat Intelligence

The lookups identified `letsdwfend.io` as malicious infrastructure associated with brand impersonation and credential-harvesting activity.

### Mail & Network Correlation

Exchange logs showed that the notification email was successfully delivered.

Proxy and network logs then showed that **Mateo's host accessed the typosquatted domain**.

This correlation is important because it connects the suspicious domain intelligence to an observed user interaction rather than treating the domain registration/MX record as an isolated indicator.

---

## 🧠 4. Analyst Assessment

### Verdict

| Assessment | Result |
| :--- | :--- |
| **Classification** | 🟢 True Positive |
| **Severity** | Medium |
| **Activity** | Phishing / Brand Impersonation |
| **Malicious Domain** | `letsdwfend[.]io` |
| **User Interaction** | Confirmed in case evidence |
| **Potential Objective** | Credential Harvesting |

### Reasoning

The alert was classified as a True Positive based on the combination of:

1. A domain designed to closely resemble the legitimate `letsdefend.io` domain.
2. Suspicious mail infrastructure associated with the domain.
3. Threat-intelligence results identifying the domain as malicious.
4. Successful delivery of the related email.
5. Network evidence showing that Mateo's host accessed the suspicious domain.

The combination of domain impersonation, malicious reputation, and confirmed user interaction provides sufficient evidence to treat the activity as a phishing-related security event.

---

## 🚨 5. Recommended SOC Response

Because this is a simulated LetsDefend investigation, the following are documented as **recommended response actions**, rather than actions performed against a production environment.

### Containment

- Remove the phishing email from affected mailboxes where appropriate.
- Block the identified typosquatted domain through DNS filtering, secure web gateways, and other appropriate controls.
- Review additional mail and proxy activity associated with the domain.
- Isolate the affected endpoint if evidence indicates potential compromise.

### Account Protection

- Verify whether the user entered credentials into the suspicious website.
- Reset credentials if credential exposure is suspected.
- Revoke active sessions where appropriate.
- Review the account for additional suspicious authentication activity.

### Endpoint Investigation

- Perform endpoint security scanning on Mateo's host.
- Review browser and endpoint telemetry for additional suspicious activity.
- Search for related indicators associated with the typosquatted domain.

---

## 🛡️ 6. Detection & Prevention Recommendations

### Typosquatting Detection

Implement or strengthen automated monitoring for newly registered or newly configured domains that closely resemble organizational or high-value third-party domains.

### Email Security

Strengthen email security controls for messages containing links to lookalike domains, especially when the message references security, authentication, or account-related activity.

### Threat Intelligence

Use domain reputation and threat-intelligence enrichment to correlate:

```text
Domain Intelligence
        +
Email Delivery
        +
User Click
        +
Network Connection
        ↓
Higher-confidence phishing detection
```

### Security Awareness

Provide targeted awareness training on identifying subtle domain differences, such as:

```text
letsdefend.io
     vs.
letsdwfend.io
```

---

## 🎯 7. MITRE ATT&CK Mapping

| Technique | ID | Relevance to Investigation |
| :--- | :--- | :--- |
| **Phishing for Information: Spearphishing Link** | **T1598.003** | The case involved an email containing a link directing the recipient toward malicious infrastructure. |
| **Phishing** | **T1566** | The suspicious activity used email delivery as part of the phishing workflow. |
| **Impersonation** | **T1656** | The infrastructure used a lookalike domain designed to resemble a legitimate organization. |

---

## 📝 8. Analyst Takeaway

This investigation demonstrates the value of **correlating threat intelligence with actual user and network activity**.

A suspicious domain alone may indicate potential abuse, but the investigation became more significant when:

```text
Typosquatted Domain
        ↓
Threat Intelligence Match
        ↓
Email Delivered
        ↓
User Click / Host Connection
        ↓
Confirmed Phishing Activity
```

The case also demonstrates why small differences in domain spelling can be important indicators during phishing investigations.

### Skills Demonstrated

- Phishing alert triage
- Typosquatting analysis
- Domain / DNS investigation
- Threat intelligence enrichment
- Email security analysis
- Proxy and network log correlation
- IOC identification
- True Positive classification
- Recommended incident response
- MITRE ATT&CK mapping

---

## ⚠️ Training Disclaimer

This case was investigated in the **LetsDefend simulated SOC environment** for cybersecurity training purposes. The infrastructure, domains, IP addresses, email addresses, hostname, event data, and investigation scenario are laboratory/training data and should not be interpreted as evidence of a real production incident handled by the analyst.
