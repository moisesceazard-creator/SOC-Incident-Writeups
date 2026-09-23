# 🚨 SOC153 - Suspicious PowerShell Script Executed

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Verdict:** 🟢 True Positive (Malicious PowerShell Script Execution & Active C2 Connection)  

---

## 📊 Executive Summary

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
| **File Hash (SHA-256)** | `db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0` |
| **C2 Domain / URL** | `hxxps[:]//kionagranada[.]com/upload/sd2.ps1` |
| **C2 IP Address** | `161.22.46.148` |
| **AV/EDR Action** | Detected / Not Quarantined |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS IR Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** The SIEM flagged an alert for `payload_1.ps1` executing via PowerShell under the user context `LetsDefend` from the `Downloads` directory.
* **Initial Observation:** Device status recorded as `Detected`, but inspection revealed the file was **Not Quarantined** by endpoint defenses, allowing execution at 05:23 PM.

### Phase 2: Identification & Analysis
* **Artifact & Indicator Triage (IoCs):**
  * **Malicious Script:** `payload_1.ps1`
  * **File Hash:** `db8be06ba6d2d3595dd0c86654a48cfc4c0c5408fdd3f4e1eaf342ac7a2479d0`
  * **Target Host:** `Tony` (`172.16.17.206`)
  * **C2 Endpoint:** `hxxps[:]//kionagranada[.]com/upload/sd2.ps1` (`161.22.46.148`)
* **Threat Intelligence & Forensics:**
  * Cross-checking the SHA-256 hash on VirusTotal confirmed the payload as malicious across multiple security vendors.
  * Log management examination showed DNS queries resolving `kionagranada.com` to `161.22.46.148`.
  * Firewall log correlation confirmed outbound HTTPS traffic to `161.22.46.148` with a status of `SUCCESS` at 05:23 PM, proving that the script successfully communicated with the Command & Control (C2) server to fetch additional payloads (`sd2.ps1`).

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Host Isolation:** Immediately isolated workstation `Tony` (`172.16.17.206`) via EDR to prevent lateral movement and interrupt active C2 communication.
  * **Perimeter Blocking:** Blacklisted destination domain `kionagranada.com` and IP `161.22.46.148` on the network firewall and web proxies.
* **Eradication:**
  * **File Removal:** Terminated associated PowerShell execution processes and permanently deleted `payload_1.ps1` from `C:\Users\LetsDefend\Downloads\`.
  * **Persistence Sweep:** Verified system startup folders, Scheduled Tasks, and Registry Run keys to confirm no secondary backdoor was dropped via `sd2.ps1`.
* **Recovery:**
  * Conducted a full anti-malware and EDR scan on host `Tony`.
  * Reconnected the endpoint to the network after confirming zero remaining IoCs.

### Phase 4: Post-Incident Activity & Lessons Learned
* **PowerShell Execution Policies:** Enforce Constrained Language Mode (CLM) and strict Execution Policies (`AllSigned` or `RemoteSigned`) via Group Policy Object (GPO).
* **Directory Execution Restrictions:** Implement Software Restriction Policies (AppLocker / Windows Defender Application Control) to block execution of script files (`.ps1`, `.vbs`, `.bat`) directly from user `Downloads` and `Temp` folders.
* **Automated Prevention Tuning:** Reconfigure EDR policies to immediately block and quarantine unverified PowerShell scripts initiating external HTTP/HTTPS connections.

---

## 🎯 MITRE ATT&CK Mapping

* **T1189 - Drive-by Compromise:** Initial vector or web download initiating payload delivery
* **T1059.001 - PowerShell:** Execution of malicious commands and scripts via Windows PowerShell
* **T1204.002 - User Execution (Malicious File):** User interaction initiating execution of downloaded script file
* **T1071 - Application Layer Protocol:** Utilizing standard web protocols (HTTPS) for Command & Control communication
