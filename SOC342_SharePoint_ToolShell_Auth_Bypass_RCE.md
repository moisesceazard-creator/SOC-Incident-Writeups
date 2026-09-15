# 🚨 SOC342 - CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Verdict:** 🟢 True Positive (Escalated to Tier 2 | 100% Playbook Score)  

---

## 📊 Executive Summary

| Field | Details |
| :--- | :--- |
| **Event ID** | 320 |
| **Severity** | Critical |
| **Category** | Web Attack / Remote Code Execution |
| **Rule Name** | SOC342 - CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE |
| **Event Time** | 2025-07-22 13:07:10 +03:00 |
| **Target Host** | `SharePoint01` (`172.16.20.17`) |
| **Source IP** | `107.191.58.76` (External / Internet) |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** Detection rule flagged an unauthenticated POST request targeting `ToolPane.aspx` with a large payload size (`Content-Length: 7699`) and a spoofed HTTP Referer header (`/_layouts/SignOut.aspx`).
* **Initial Observation:** Request originated externally from IP `107.191.58.76` directly targeting the internal SharePoint server `SharePoint01` (`172.16.20.17`). Device action recorded as `Allowed`.

### Phase 2: Identification & Analysis
* **Artifact & Indicator Triage (IoCs):**
  * **Source IP Address:** `107.191.58.76`
  * **Target Host / IP:** `SharePoint01` (`172.16.20.17`)
  * **Requested URL:** `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx`
  * **HTTP Referer:** `/_layouts/SignOut.aspx`
  * **HTTP Method:** POST
  * **User-Agent:** `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:120.0) Gecko/20100101 Firefox/120.0`
* **Vulnerability Analysis:** Confirmed exploitation of **CVE-2025-53770** ("ToolShell"), an unsafe deserialization flaw in on-premises SharePoint Server deployments that enables authentication bypass and Remote Code Execution (RCE).
* **Exploit Verification:** Web server logs returned an HTTP `200 OK` status code, confirming the authentication bypass and command execution were successfully processed by the web application layer.
* **Authorization Check:** Cross-referenced source IP and target host against scheduled penetration tests and internal email logs; verified as an unauthorized, malicious attack.

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Host Isolation:** Isolated `SharePoint01` (`172.16.20.17`) from the internal network via EDR to halt post-exploitation activity and lateral movement.
  * **Perimeter Blocking:** Blacklisted adversary IP `107.191.58.76` across perimeter firewalls and Web Application Firewalls (WAF).
  * **Tier 2 Escalation:** Escalated case to Tier 2 Incident Response for forensic memory acquisition and webshell inspection across SharePoint IIS directories.
* **Eradication:**
  * **Webshell & Process Cleanup:** Terminated active rogue worker processes and purged spawned webshells/payloads in the IIS web root.
  * **Credential Invalidation:** Reset IIS app pool service account credentials and privileged SharePoint administrative accounts.
* **Recovery:**
  * **Patch Management:** Deployed emergency vendor security updates addressing the ToolShell vulnerability (CVE-2025-53770).
  * **System Integrity Verification:** Executed full endpoint anti-malware and file integrity checks prior to restoring `SharePoint01` to active production.

### Phase 4: Post-Incident Activity & Lessons Learned
* **WAF Inspection Gaps:** Because the exploit payload passed through (`Allowed`), Web Application Firewall rules must be updated with dynamic inspection signatures for `ToolPane.aspx` parameter tampering and deserialization payloads.
* **On-Premises SharePoint Hardening:** Zero-day vulnerabilities in public-facing productivity suites require rapid virtual patching or temporary endpoint restriction until official patches can be verified.
* **Egress & Post-Exploitation Monitoring:** Immediate isolation prevented secondary lateral reconnaissance commands (`whoami`, `net group`, WMI queries) from establishing persistent reverse shells.

---

## 🎯 MITRE ATT&CK Mapping

* **T1190 - Exploit Public-Facing Application:** Exploitation of CVE-2025-53770 on SharePoint server
* **T1059.003 - Command and Scripting Interpreter (Windows Command Shell):** Command execution post-deserialization
* **T1505.003 - Server Software Component (Web Shell):** Webshell persistence via IIS web root
* **T1047 - Windows Management Instrumentation:** System management execution and discovery
* **T1082 - System Information Discovery:** System configuration probing
* **T1083 - File and Directory Discovery:** File system enumeration post-compromise
