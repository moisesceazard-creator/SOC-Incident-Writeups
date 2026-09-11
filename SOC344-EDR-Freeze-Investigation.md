# 🔍 SOC Incident Write-Up: SOC344 - EDR Tampering Attempt via EDR-Freeze

**Platform:** LetsDefend  
**Case Name:** SOC344 - EDR Tampering Attempt via EDR-Freeze  
**Event ID:** 322 | **Severity:** High | **Difficulty:** Medium  
**Analyst:** Moises Ceazar Del Mundo, CC  
**Target Host:** WS-Prod-02 (`172.16.20.69`)  

---

## 1. Executive Summary

A high-severity alert triggered on host **WS-Prod-02** (`172.16.20.69`) detecting an unauthorized attempt to execute `EDR-Freeze_1.0.exe`. Investigation confirmed that an external attacker gained access via RDP brute-force activity, conducted PowerShell reconnaissance, and subsequently attempted to blind endpoint defenses using an EDR suspension/tampering utility. The incident was classified as a **True Positive**, the threat binary was isolated, and containment actions were implemented.

---

## 2. Alert & Telemetry Breakdown

| Field | Incident Value |
| :--- | :--- |
| **Rule Name** | `SOC344 - EDR Tampering Attempt via EDR-Freeze` |
| **Event Time** | `2025-09-26 17:26:44 UTC+3` |
| **Hostname / IP** | `WS-Prod-02` / `172.16.20.69` |
| **Process Path** | `C:\Users\LetsDefend\Downloads\EDR-Freeze_1.0.exe` |
| **Command Line** | `"C:\Users\LetsDefend\Downloads\EDR-Freeze_1.0.exe" 6080 10000` |
| **SHA256 Hash** | `970c7834e58b6ef22473875167a333dbb33bf7b667d1cb814829f68579cd85f7` |

---

## 3. Attack Chain & Investigation Workflow

### Step 1: Initial Access Verification
* **RDP Brute-Force Triage:** Reviewed authentication logs prior to the alert event. Identified multiple failed RDP login attempts originating from an external source followed by a successful logon event to `WS-Prod-02`.

### Step 2: Reconnaissance Activity
* **PowerShell Telemetry:** Following initial access, the attacker executed reconnaissance commands using PowerShell to map local users, network shares, and active running security processes.

### Step 3: Defense Evasion & EDR Tampering
* **Execution:** The attacker transferred and executed `EDR-Freeze_1.0.exe` from the user's `Downloads` folder.
* **Argument Analysis:** Executed with parameters `6080 10000` (`[Target_PID] [Duration_ms]`), attempting to freeze/suspend the target EDR process PID `6080` for 10,000 milliseconds to evade detection.
* **Threat Intelligence Lookup:** Queried VirusTotal using SHA256 hash `970c7834e58b6ef22473875167a333dbb33bf7b667d1cb814829f68579cd85f7`. The binary returned multiple vendor detections classifying it as an EDR suspension/tampering hacktool.

---

## 4. Indicators of Compromise (IOCs)

* **Malicious File:** `EDR-Freeze_1.0.exe`
* **File Path:** `C:\Users\LetsDefend\Downloads\EDR-Freeze_1.0.exe`
* **SHA256 Hash:** `970c7834e58b6ef22473875167a333dbb33bf7b667d1cb814829f68579cd85f7`
* **Affected IP:** `172.16.20.69`

---

## 5. MITRE ATT&CK Mapping

* **T1078 - Valid Accounts:** Initial compromise via successful RDP authentication following brute-force.
* **T1059.001 - Command and Scripting Interpreter: PowerShell:** Execution of system reconnaissance commands.
* **T1562.001 - Impair Defenses: Disable or Modify Tools:** Attempting to freeze/disable EDR process execution.
* **T1055 - Process Injection / T1489 - Service Stop:** Tampering with underlying system security components.

---

## 6. Final Verdict & Playbook Remediation

* **Final Verdict:** **True Positive**
* **Remediation Actions Taken:**
  1. **Host Isolation:** Immediately isolated `WS-Prod-02` (`172.16.20.69`) from the network to prevent lateral movement.
  2. **Process Termination:** Terminated process PID `6080` and killed `EDR-Freeze_1.0.exe`.
  3. **Artifact Removal:** Deleted `EDR-Freeze_1.0.exe` from `C:\Users\LetsDefend\Downloads\`.
  4. **Credential Reset:** Forcefully reset credentials for the compromised user account and revoked active RDP sessions.
  5. **Network Hardening:** Restricted external RDP access (Port 3389) via perimeter firewall rules and recommended enforcing NLA + Multi-Factor Authentication (MFA).
