# SOC342 — CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE

**Analyst:** Moises Ceazar Del Mundo, CC  
**Platform:** LetsDefend  
**Environment:** Simulated SOC investigation  
**Verdict:** True Positive — Authentication Bypass / Remote Code Execution  
**Playbook Result:** 100%

> **Training Context:** This investigation was performed in the LetsDefend training environment. The IP addresses, hosts, requests, and other indicators documented here are lab/training data and do not represent a real production incident.

---

## 1. Incident Overview

| Field | Details |
|---|---|
| Event ID | 320 |
| Severity | Critical |
| Category | Web Attack / Remote Code Execution |
| Detection Rule | SOC342 — CVE-2025-53770 SharePoint ToolShell Auth Bypass and RCE |
| Event Time | 2025-07-22 13:07:10 +03:00 |
| Target Host | `SharePoint01` (`172.16.20.17`) |
| Source IP | `107.191.58.76` |
| HTTP Method | POST |
| Requested URL | `/_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx` |
| HTTP Referer | `/_layouts/SignOut.aspx` |
| User-Agent | Firefox 120 on Windows |
| Result | True Positive |

---

## 2. Detection & Initial Triage

The detection rule flagged an unauthenticated HTTP POST request targeting the SharePoint `ToolPane.aspx` endpoint.

Several characteristics made the request suspicious:

- External source IP directly targeting the SharePoint server.
- Unauthenticated request.
- Large request payload (`Content-Length: 7699`).
- Spoofed HTTP Referer: `/_layouts/SignOut.aspx`.
- Targeting of the `ToolPane.aspx` functionality associated with the ToolShell exploitation path.
- Device action recorded as **Allowed**.

The request was therefore escalated for vulnerability and exploit analysis.

---

## 3. Exploit & Artifact Analysis

### Request Indicators

```text
Source IP:
107.191.58.76

Target:
SharePoint01 (172.16.20.17)

Method:
POST

Endpoint:
 /_layouts/15/ToolPane.aspx?DisplayMode=Edit&a=/ToolPane.aspx

Referer:
 /_layouts/SignOut.aspx
```

The case identifies the activity as exploitation of **CVE-2025-53770 (ToolShell)**, an unsafe deserialization vulnerability affecting on-premises SharePoint Server deployments.

The vulnerability can enable authentication bypass and remote code execution.

### Exploit Verification

The web server returned:

```text
HTTP 200 OK
```

The LetsDefend case interprets this response as confirmation that the malicious request was successfully processed by the web application layer.

The source IP and target host were also checked against scheduled penetration tests and internal email records. The activity was determined to be **unauthorized and malicious**.

---

## 4. Analyst Assessment

**Final Verdict: TRUE POSITIVE**

The evidence supports classification as a successful exploitation attempt against a public-facing SharePoint service.

The investigation correlated:

1. An external source directly targeting the SharePoint server.
2. An unauthenticated POST request.
3. A suspicious request to `ToolPane.aspx`.
4. A spoofed Referer header.
5. A large request payload consistent with exploit activity.
6. An HTTP 200 response indicating successful processing.
7. Case evidence identifying the activity as CVE-2025-53770 exploitation.
8. Validation that the source was not associated with an authorized penetration test.

This combination justified the Critical severity and Tier 2 escalation in the LetsDefend scenario.

---

## 5. Recommended SOC Response

The original LetsDefend scenario describes host isolation, perimeter blocking, webshell/process cleanup, credential resets, emergency patching, and integrity verification.

For portfolio accuracy, these actions are presented below as **recommended SOC response actions**, not as production actions personally executed by the analyst.

### Recommended Containment

- Restrict external access to the affected SharePoint service where operationally feasible.
- Block the confirmed malicious source IP through appropriate perimeter controls.
- Preserve relevant IIS, SharePoint, network, and endpoint telemetry for forensic analysis.
- Escalate to Tier 2 / Incident Response for deeper compromise assessment.

### Recommended Eradication

- Inspect SharePoint and IIS directories for unauthorized webshells or dropped payloads.
- Review suspicious worker processes and child processes spawned by the SharePoint service.
- Investigate authentication and privileged-account activity following the exploitation window.
- Remove confirmed malicious artifacts after evidence preservation.

### Recommended Recovery

- Apply the appropriate vendor security updates addressing CVE-2025-53770.
- Validate system and application integrity.
- Review SharePoint service-account and administrative credentials if compromise is confirmed or suspected.
- Restore normal exposure only after vulnerability remediation and compromise assessment are complete.

---

## 6. Detection & Prevention Recommendations

### Web Application / WAF Monitoring

Create detections for:

- Suspicious requests to `ToolPane.aspx`.
- Unexpected unauthenticated access to administrative SharePoint functionality.
- Abnormal request sizes or parameter combinations.
- Suspicious Referer manipulation.
- Deserialization-related payload patterns.

### Exposure Management

Public-facing SharePoint deployments should be prioritized for rapid vulnerability remediation.

When a critical vulnerability affects an internet-exposed application, temporary access restrictions or virtual patching can reduce exposure while permanent remediation is being prepared.

### Post-Exploitation Monitoring

Monitor SharePoint/IIS servers for:

- Unexpected child processes.
- Webshell creation.
- Command-shell or PowerShell execution.
- Unusual outbound network connections.
- New or modified files within web application directories.

---

## 7. MITRE ATT&CK Mapping

| Technique | ID | Relevance |
|---|---|---|
| Exploit Public-Facing Application | T1190 | The case identifies exploitation of CVE-2025-53770 against an externally accessible SharePoint service. |
| Command and Scripting Interpreter: Windows Command Shell | T1059.003 | The original case maps post-exploitation command execution to Windows command shell activity. |
| Server Software Component: Web Shell | T1505.003 | The original case identifies webshell persistence through the IIS web root. |
| Windows Management Instrumentation | T1047 | The original case references WMI activity during post-exploitation. |
| System Information Discovery | T1082 | The original case references system-configuration probing. |
| File and Directory Discovery | T1083 | The original case references filesystem enumeration after compromise. |

**Evidence note:** T1190 is the strongest directly supported mapping from the exploit evidence. The remaining post-exploitation techniques are retained because they are explicitly referenced in the LetsDefend scenario, but the available event excerpt does not independently provide the underlying command or webshell artifacts.

---

## 8. Analyst Takeaway

This case demonstrates the importance of treating exploitation alerts against public-facing applications as more than simple vulnerability detections.

The key investigation path was:

**external request → suspicious SharePoint endpoint → exploit-pattern analysis → HTTP response verification → authorization check → Tier 2 escalation.**

A SOC analyst should correlate the HTTP request with application, endpoint, and network telemetry to determine whether the activity represents scanning, an unsuccessful exploit attempt, or successful exploitation.

For a Tier 1 SOC analyst, this case demonstrates **web-attack triage, exploit analysis, IOC extraction, HTTP log analysis, vulnerability assessment, incident validation, severity assessment, and escalation to Tier 2.**

---

## 9. Skills Demonstrated

- SOC alert triage
- Web attack investigation
- HTTP request analysis
- Exploit detection
- CVE analysis
- IOC extraction
- Log correlation
- Public-facing application security
- Incident severity assessment
- Tier 2 escalation
- Recommended containment planning
- MITRE ATT&CK mapping

---

## Training Disclaimer

This case study documents a **LetsDefend simulated SOC investigation**. It should be interpreted as hands-on cybersecurity training rather than professional production SOC experience.

The response recommendations are presented as analyst recommendations based on the evidence available in the exercise. No claim is made that these actions were executed against a real production environment.
