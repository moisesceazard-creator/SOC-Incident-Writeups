# 🚨 SOC257 — VPN Connection Detected from Unauthorized Country

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Environment:** Simulated SOC training environment  
**Verdict:** 🟢 True Positive — Unauthorized VPN Connection Attempt  

> **Training Context:** This investigation is based on a simulated SOC alert from the LetsDefend lab environment. The IOCs, IP addresses, user information, and investigation details are training data and do not represent a production incident handled by the analyst.

---

## 📊 1. Incident Overview

| Field | Details |
| :--- | :--- |
| **Event ID** | 225 |
| **Severity** | Low |
| **Category** | Unauthorized Access |
| **Rule Name** | SOC257 - VPN Connection Detected from Unauthorized Country |
| **Event Time** | 2024-02-13 02:04:00 +03:00 |
| **Target User** | `monica@letsdefend.io` |
| **Source IP** | `113.161.158.12` |
| **Destination IP / Service** | `33.33.33.33` (`https://vpn-letsdefend.io`) |
| **Verdict** | True Positive |
| **Escalation** | Based on investigation findings |

---

## 🔎 2. Detection & Initial Triage

### Alert Trigger

The detection rule identified a VPN authentication attempt targeting:

```text
https://vpn-letsdefend.io
```

from the source IP:

```text
113.161.158.12
```

The connection originated from an unauthorized geographic location and targeted the account:

```text
monica@letsdefend.io
```

### Initial Observations

- The source originated from an unauthorized country/location.
- The activity targeted an enterprise VPN service.
- The targeted account was a valid user account.
- Authentication logs showed successful primary credential validation.
- MFA authentication subsequently failed.

The combination of an anomalous geographic source and successful primary credential validation warranted further investigation.

---

## 🧩 3. Authentication & Evidence Analysis

### Indicators Identified

| Indicator | Type | Observation |
| :--- | :--- | :--- |
| `monica@letsdefend.io` | User Account | Targeted account |
| `113.161.158.12` | Source IP | Origin of the suspicious VPN authentication attempt |
| `33.33.33.33` | Destination IP | VPN service destination |
| `https://vpn-letsdefend.io` | Service | Targeted VPN endpoint |
| `Incorrect OTP Code` | Authentication Event | Repeated MFA failures |

### Authentication Sequence

The authentication logs showed the following sequence:

```text
1. Primary username/password credentials matched
                    ↓
2. MFA challenge presented
                    ↓
3. Incorrect OTP entered
                    ↓
4. Additional incorrect OTP attempt
                    ↓
5. Successful VPN session was not established
```

### Key Finding

The actor was able to successfully match the user's primary credentials, but the subsequent MFA step failed.

This distinction is important:

> **Credential validation succeeded, but the authentication process did not reach a successful authenticated VPN session because MFA was not successfully completed.**

Therefore, the evidence supports classification as an **unauthorized VPN connection attempt**, rather than a confirmed successful VPN compromise.

---

## 🧠 4. Analyst Assessment

### Verdict

| Assessment | Result |
| :--- | :--- |
| **Classification** | 🟢 True Positive |
| **Severity** | Low |
| **Activity** | Unauthorized VPN connection attempt |
| **Primary Credential Validation** | Successful |
| **MFA** | Failed |
| **Successful VPN Session** | Not established |

### Reasoning

The alert was classified as a True Positive based on:

1. The VPN authentication originated from an unauthorized geographic location.
2. The activity targeted a valid user account.
3. The supplied primary credentials were successfully matched.
4. MFA subsequently failed because of incorrect OTP submissions.
5. No successful VPN session was established.

The successful primary credential validation is the most significant finding because it indicates that the account password was potentially exposed or otherwise known to the actor, while MFA prevented completion of the login process.

---

## 🚨 5. Recommended SOC Response

Because this is a simulated LetsDefend investigation, the following are documented as **recommended response actions**, rather than actions performed against a production environment.

### Containment

- Reset the affected user's primary credentials.
- Revoke active sessions associated with the account.
- Block the identified source IP where appropriate.
- Review other authentication attempts originating from the same source.

### Account & Authentication Investigation

- Verify whether the user recognizes the login attempt.
- Review recent authentication history for additional anomalous activity.
- Review the affected account for signs of credential reuse or compromise.
- Re-register or reset MFA factors if compromise of the authentication device is suspected.

### Recovery

- Confirm that the user can securely authenticate after credential reset.
- Continue monitoring the account for additional suspicious authentication attempts.
- Review conditional-access or geo-restriction policies for similar activity.

---

## 📝 6. Post-Incident Observations

### MFA as a Security Control

The case demonstrates the value of MFA as a defense-in-depth control. Although the primary credentials were successfully matched, the failed OTP prevented the authentication flow from reaching a successful VPN session.

### Credential Security

The successful primary credential validation warrants investigation into how the credentials may have been exposed. Possible areas for investigation include credential reuse, phishing, credential stuffing, or other prior credential exposure.

> These are investigation hypotheses; the case evidence establishes successful credential validation but does not by itself prove the original source of the credential compromise.

### Geographic Access Controls

Unauthorized geographic access can provide a useful detection signal for VPN monitoring. Conditional-access or geo-restriction controls may help reduce exposure from unexpected locations, depending on the organization's legitimate travel and remote-access requirements.

---

## 🎯 7. MITRE ATT&CK Mapping

| Technique | ID | Relevance to Investigation |
| :--- | :--- | :--- |
| **External Remote Services** | **T1133** | The suspicious activity targeted an external enterprise VPN service. |
| **Multi-Factor Authentication Request Generation** | **T1621** | The actor generated MFA challenges and submitted incorrect OTP values after primary credentials were accepted. |

### Mapping Note

The original case also referenced **T1595 — Active Scanning**. However, the evidence documented in this write-up directly establishes an authentication attempt and MFA activity, not independent scanning behavior. For portfolio accuracy, the primary mapping is therefore kept focused on the observed VPN authentication workflow.

---

## 📝 8. Analyst Takeaway

This investigation demonstrates how a SOC analyst should distinguish between:

**valid credentials → successful authentication**

and

**valid credentials → failed MFA → unsuccessful login.**

The most important finding was not simply the unusual country. The investigation showed that the actor possessed or successfully supplied valid primary credentials, while MFA acted as the control preventing completion of the VPN login.

### Skills Demonstrated

- VPN alert triage
- Authentication log analysis
- Account compromise assessment
- MFA investigation
- Geographic anomaly analysis
- True Positive classification
- Incident escalation
- Recommended containment planning
- MITRE ATT&CK mapping

---

## ⚠️ Training Disclaimer

This case was investigated in the **LetsDefend simulated SOC environment** for cybersecurity training purposes. The infrastructure, IP addresses, user accounts, event data, and investigation scenario are laboratory/training data and should not be interpreted as evidence of a real production incident handled by the analyst.
