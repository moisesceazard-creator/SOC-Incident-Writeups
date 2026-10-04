# 🚨 SOC153 — Suspicious PowerShell Script Executed

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Environment:** Simulated SOC training environment  
**Verdict:** 🟢 True Positive — Malicious PowerShell Execution & Active C2 Communication  

> **Training Context:** This investigation is based on a simulated SOC alert from the LetsDefend lab environment. The IOCs, IP addresses, host information, user account, and investigation details are training data and do not represent a production incident handled by the analyst.

---

## 📊 1. Incident Overview

| Field | Details |
| :--- | :--- |
| **Event ID** | 238 |
| **Severity** | Medium |
| **Category** | Malware |
| **Rule Name** | SOC153 - Suspicious Powershell Script Executed |
| **Event Time** | 2024-03-14 17:23:43 +03:00 |
| **Target Host** | `Tony` (`172.16.17.206`) |
| **User Account** | `LetsDefend` |
| **File Name** | `payload_1.ps1` |
| **File Path** | `C:\Users\LetsDefend\Downloads\payload_1.ps1` |
| **SHA-256** | `db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0` |
| **C2 Domain / URL** | `hxxps[:]//kionagranada[.]com/upload/sd2.ps1` |
| **C2 IP Address** | `161.22.46.148` |
| **AV/EDR Action** | Detected / Not Quarantined |
| **Verdict** | True Positive |

---

## 🔎 2. Detection & Initial Triage

### Alert Trigger

The SIEM generated an alert after `payload_1.ps1` was executed through PowerShell under the `LetsDefend` user context from the user's Downloads directory.

### Initial Observations

- A PowerShell script was executed from a user-controlled Downloads directory.
- Endpoint defenses detected the file but did **not quarantine it**.
- The script therefore remained available for execution.
- The alert required further investigation to determine whether the script was malicious and whether external communication occurred.

The combination of PowerShell execution, an unusual script artifact, and the lack of quarantine warranted deeper analysis.

---

## 🧩 3. Artifact & IOC Analysis

### Indicators Identified

| Indicator | Type | Observation |
| :--- | :--- | :--- |
| `payload_1.ps1` | PowerShell Script | Suspicious script executed on the endpoint |
| `db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0` | SHA-256 | Confirmed malicious through VirusTotal |
| `kionagranada.com` | Domain | C2-related domain |
| `161.22.46.148` | IP Address | Resolved destination / C2 address |
| `/upload/sd2.ps1` | URL Path | Additional PowerShell payload referenced by the C2 endpoint |
| `Tony` / `172.16.17.206` | Host | Affected endpoint |

### Threat Intelligence

The SHA-256 hash of `payload_1.ps1` was checked against VirusTotal and was identified as malicious by multiple security vendors.

This provided independent threat-intelligence support for classifying the PowerShell script as malicious.

### DNS & Network Correlation

Log analysis showed DNS resolution of:

```text
kionagranada.com → 161.22.46.148
```

Firewall logs then showed successful outbound HTTPS communication to:

```text
161.22.46.148
```

at approximately 05:23 PM.

The network evidence is significant because it connects the malicious PowerShell execution with external communication to the identified C2 infrastructure. The case evidence further indicates that the connection was used to retrieve an additional PowerShell payload:

```text
sd2.ps1
```

---

## 🔬 4. Investigation Timeline

```text
PowerShell script detected
        ↓
payload_1.ps1 identified
        ↓
SHA-256 analyzed through VirusTotal
        ↓
Payload confirmed malicious
        ↓
DNS query identified for kionagranada.com
        ↓
Domain resolved to 161.22.46.148
        ↓
Successful outbound HTTPS connection observed
        ↓
C2 communication / additional payload retrieval identified
        ↓
Incident classified as True Positive
```

This sequence connects the endpoint artifact, threat intelligence evidence, and network telemetry into a single investigation narrative.

---

## 🧠 5. Analyst Assessment

### Verdict

| Assessment | Result |
| :--- | :--- |
| **Classification** | 🟢 True Positive |
| **Severity** | Medium |
| **Malicious Artifact** | `payload_1.ps1` |
| **Execution Method** | PowerShell |
| **C2 Communication** | Confirmed in case evidence |
| **Additional Payload** | `sd2.ps1` |
| **Escalation** | Incident response / containment required |

### Reasoning

The alert was classified as a True Positive based on multiple correlated findings:

1. A PowerShell script was executed from the user's Downloads directory.
2. Endpoint defenses detected the file but did not quarantine it.
3. The file hash was identified as malicious through VirusTotal.
4. DNS telemetry linked the script's activity to `kionagranada.com`.
5. The domain resolved to `161.22.46.148`.
6. Firewall logs recorded successful outbound HTTPS communication to the identified IP.
7. The case evidence indicates that the connection was used to retrieve an additional PowerShell payload.

Taken together, these findings demonstrate malicious script execution accompanied by active external C2 communication.

---

## 🚨 6. Recommended SOC Response

Because this is a simulated LetsDefend investigation, the following are documented as **recommended response actions**, rather than actions performed against a production environment.

### Containment

- Isolate the affected endpoint `Tony` from the network to interrupt potential C2 communication and reduce the risk of further activity.
- Block the identified C2 domain and IP address through appropriate security controls.
- Review other endpoints for communication with the same infrastructure.

### Eradication & Investigation

- Terminate malicious PowerShell processes associated with the identified script.
- Remove confirmed malicious artifacts after appropriate evidence preservation.
- Investigate whether `sd2.ps1` or additional payloads were downloaded or executed.
- Review persistence locations such as Scheduled Tasks, startup locations, and relevant Registry Run keys.
- Conduct additional endpoint scanning and forensic review for related indicators.

### Recovery

- Confirm that the endpoint is free of identified malicious artifacts and related indicators before restoring normal network access.
- Continue monitoring the endpoint and associated IOCs for recurring activity.

---

## 🛡️ 7. Detection & Prevention Recommendations

The case highlights several defensive opportunities:

### PowerShell Controls

- Apply appropriate PowerShell security controls such as Constrained Language Mode where operationally suitable.
- Use appropriate execution-policy controls and application-control mechanisms to reduce unauthorized script execution.

### Script Execution Restrictions

- Consider application-control policies such as AppLocker or Windows Defender Application Control to restrict untrusted script execution from user-writable directories such as Downloads and Temp.

### EDR/SIEM Detection

- Tune endpoint detection to identify suspicious PowerShell execution combined with external HTTP/HTTPS communication.
- Alert on PowerShell scripts retrieving additional scripts or payloads from external infrastructure.
- Correlate endpoint execution telemetry with DNS and network connection data to improve detection confidence.

---

## 🎯 8. MITRE ATT&CK Mapping

| Technique | ID | Relevance to Investigation |
| :--- | :--- | :--- |
| **PowerShell** | **T1059.001** | `payload_1.ps1` was executed through Windows PowerShell. |
| **User Execution: Malicious File** | **T1204.002** | The case associates execution with a downloaded PowerShell script in the user's Downloads directory. |
| **Application Layer Protocol** | **T1071** | HTTPS was used for external communication associated with the malicious activity. |
| **Drive-by Compromise** | **T1189** | Referenced by the original case as a possible initial payload-delivery vector. The available case evidence does not independently establish the initial delivery mechanism. |

> **Mapping Note:** MITRE mappings are limited to techniques supported or referenced by the LetsDefend case. `T1189` is retained as a case-referenced technique but should not be interpreted as proof of how the initial file was delivered.

---

## 📝 9. Analyst Takeaway

This investigation demonstrates why a suspicious PowerShell alert should not be evaluated in isolation.

The strongest finding came from **correlating multiple sources of evidence**:

```text
Endpoint Execution
       +
File Hash Reputation
       +
DNS Resolution
       +
Firewall Traffic
       ↓
High-confidence malicious activity
```

The investigation moved from a single suspicious PowerShell execution to a broader assessment of **malicious code execution and active C2 communication**.

### Skills Demonstrated

- SOC alert triage
- PowerShell investigation
- Malware artifact analysis
- SHA-256 / IOC analysis
- VirusTotal threat intelligence
- DNS analysis
- Firewall/network log correlation
- C2 identification
- Incident classification
- Recommended containment planning
- MITRE ATT&CK mapping

---

## ⚠️ Training Disclaimer

This case was investigated in the **LetsDefend simulated SOC environment** for cybersecurity training purposes. The infrastructure, IP addresses, hostname, user account, event data, and investigation scenario are laboratory/training data and should not be interpreted as evidence of a real production incident handled by the analyst.
