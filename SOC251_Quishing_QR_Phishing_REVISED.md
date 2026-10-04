# SOC251 — Quishing Detected (QR Code Phishing)

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Environment:** Simulated SOC investigation  
**Verdict:** True Positive — QR Code Phishing / Quishing

> **Training Context:** This investigation was performed in the LetsDefend training environment. The email addresses, domains, IP addresses, hosts, and other indicators documented here are lab/training data and do not represent a real production incident.

---

## 1. Incident Overview

| Field | Details |
|---|---|
| Event ID | 214 |
| Severity | Medium |
| Category | Phishing / Quishing |
| Detection Rule | SOC251 — Quishing Detected (QR Code Phishing) |
| Event Time | 2024-01-01 12:37:38 +03:00 |
| Target User | `Claire@letsdefend.io` |
| Target Host | `172.16.17.72` |
| Sender | `security@microsecmfa.com` |
| Source IP | `158.69.201.47` |
| Subject | `New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA)` |
| Device Action | Allowed |
| Final Assessment | True Positive |
| Playbook Result | 100% |

---

## 2. Detection & Initial Triage

The alert identified a phishing message using a **QR code** as the primary phishing vector.

The email used a security-themed lure, claiming to be a mandatory MFA security update. This type of message can create urgency and encourage the recipient to scan the embedded QR code.

The sender domain `microsecmfa.com` was identified as a **typosquatting domain impersonating Microsoft MFA**, making the sender identity a significant initial indicator.

### Initial Indicators

- Suspicious sender domain: `microsecmfa.com`
- Source IP: `158.69.201.47`
- Target user: `Claire@letsdefend.io`
- Target host: `172.16.17.72`
- MFA/security-themed social-engineering lure
- Embedded QR code
- External phishing infrastructure

The combination of sender impersonation and an embedded QR code justified deeper investigation.

---

## 3. Domain & Threat Intelligence Analysis

The domain `microsecmfa.com` was identified as typosquatting Microsoft MFA.

Threat-intelligence information also flagged the source IP `158.69.201.47` as associated with **active credential-harvesting infrastructure**.

These indicators strengthened the assessment that the email was designed to impersonate a legitimate MFA/security service and redirect the recipient toward a credential-harvesting page.

---

## 4. QR Code Analysis

Because the phishing vector was an embedded QR code, the QR content had to be decoded and analyzed rather than relying only on the visible email text.

The embedded QR code was decoded successfully.

Analysis showed that the QR code redirected the user to an **external landing page imitating an MFA login prompt**.

This established the intended phishing flow:

```text
Security-Themed Phishing Email
          ↓
       QR Code
          ↓
    External Redirect
          ↓
   Fake MFA Login Page
          ↓
Potential Credential Harvesting
```

The QR code therefore functioned as the delivery mechanism for the phishing URL.

---

## 5. Log Correlation & Scope Assessment

Log-management data showed that communication associated with the phishing activity was limited to the target host:

```text
Claire@letsdefend.io
        ↓
172.16.17.72
```

No broader internal spread was identified in the available evidence.

This was important for scoping the incident. The available logs supported a targeted phishing interaction rather than evidence of widespread internal propagation.

---

## 6. Analyst Assessment

**Final Verdict: TRUE POSITIVE**

The evidence supports classification of this alert as a legitimate **QR-code phishing / quishing attempt**.

The investigation correlated:

1. A security-themed phishing lure.
2. A sender domain designed to resemble Microsoft MFA.
3. Threat-intelligence findings associated with the source infrastructure.
4. An embedded QR code.
5. Successful QR decoding.
6. Redirection to an external page imitating an MFA login prompt.
7. Log evidence showing communication limited to the targeted host.

The combination of these indicators provides strong evidence of a credential-harvesting phishing campaign.

---

## 7. Recommended SOC Response

The original LetsDefend scenario describes actions such as blocking the source/domain, revoking sessions, resetting credentials, purging the phishing email, and verifying the affected account.

For portfolio accuracy, these are presented here as **recommended SOC response actions**, not as production actions personally executed by the analyst.

### Recommended Containment

- Block the identified malicious domain and source infrastructure through appropriate security controls.
- Remove or quarantine the phishing message from affected mailboxes.
- Search for the same sender, domain, URL, and QR-based lure across other mailboxes.
- Investigate whether the targeted user submitted credentials to the fake MFA page.

### Recommended Credential Protection

If credential submission is confirmed or suspected:

- Reset the affected user's credentials.
- Revoke active authentication sessions.
- Review MFA configuration and authentication activity.
- Check for suspicious sign-ins following the phishing interaction.

### Recommended Follow-Up

- Preserve relevant email and network evidence for further investigation.
- Search for additional users who received the same campaign.
- Add confirmed malicious indicators to appropriate detection/blocking controls.
- Review whether QR-code content is adequately inspected by existing email-security controls.

---

## 8. Detection & Prevention Recommendations

### QR-Code / Quishing Detection

Traditional email inspection can miss malicious URLs embedded inside QR codes because the URL is not necessarily visible as normal text.

SOC monitoring should therefore consider:

- QR-code extraction and URL inspection where supported.
- Detection of QR codes in unsolicited authentication/security emails.
- Correlation between email delivery and subsequent external connections.
- Analysis of redirected authentication pages.

### Typosquatting Detection

Improve detection of domains that visually resemble trusted authentication providers.

For this case, the suspicious similarity of:

```text
microsecmfa.com
```

to Microsoft MFA-related branding was an important indicator.

### User Awareness

Users should be cautious when asked to scan QR codes for unexpected security or MFA changes.

A legitimate-looking security message should not automatically be trusted, especially when it creates urgency or requests authentication through an unfamiliar domain.

---

## 9. MITRE ATT&CK Mapping

| Technique | ID | Relevance |
|---|---|---|
| Gather Victim Identity Information: Email Addresses | T1589.002 | Referenced in the original case as part of the adversary's targeting activity. |
| Phishing for Information: Spearphishing Link | T1598.003 | The QR code redirected the victim to a phishing page designed to imitate an MFA login prompt. |
| Masquerading | T1036 | The sender/domain used Microsoft MFA-related impersonation. |
| Deobfuscate/Decode Files or Information | T1140 | The embedded QR code was decoded to reveal the phishing destination. |

**Evidence note:** The strongest directly demonstrated techniques in this investigation are the phishing/credential-harvesting flow, masquerading, and QR-code decoding. The T1589.002 mapping is retained because it is referenced by the original LetsDefend case, but the available evidence does not independently demonstrate collection of email addresses by the attacker.

---

## 10. Analyst Takeaway

This case demonstrates why **quishing requires a different inspection mindset from conventional email phishing**.

The malicious destination was hidden inside a QR code rather than presented directly as a visible URL. The investigation therefore required decoding the QR content, validating the destination, checking threat intelligence, and correlating the resulting activity with endpoint/network logs.

The key investigation chain was:

**security-themed lure → QR code → external redirect → fake MFA login page → potential credential harvesting.**

For a Tier 1 SOC analyst, this case demonstrates **phishing triage, QR-code analysis, domain impersonation detection, threat-intelligence validation, IOC analysis, log correlation, incident scoping, and MITRE ATT&CK mapping.**

---

## 11. Skills Demonstrated

- SOC alert triage
- QR-code / quishing analysis
- Phishing investigation
- Typosquatting identification
- Threat-intelligence analysis
- IOC extraction
- Domain reputation analysis
- Log correlation
- Incident scoping
- Credential-phishing analysis
- MITRE ATT&CK mapping
- Recommended containment planning

---

## Training Disclaimer

This case study documents a **LetsDefend simulated SOC investigation**. It should be interpreted as hands-on cybersecurity training rather than professional production SOC experience.

The response recommendations are presented as analyst recommendations based on the evidence available in the exercise. No claim is made that these actions were executed against a real production environment.
