# 🚨 SOC257 - VPN Connection Detected from Unauthorized Country

**Analyst:** Moises Ceazar Del Mundo, CC 
**Platform:** LetsDefend 
**Verdict:** 🟢 True Positive (Unauthorized VPN Connection Attempt) 

---

## 📊 Executive Summary

| Field | Details |
| :--- | :--- |
| **Event ID** | 225 |
| **Severity** | Low |
| **Category** | Unauthorized Access |
| **Rule Name** | SOC257 - VPN Connection Detected from Unauthorized Country |
| **Event Time** | 2024-02-13 02:04:00 +03:00 |
| **Target User** | `monica@letsdefend.io` |
| **Source IP** | `113.161.158.12` |
| **Destination IP** | `33.33.33.33` (`https://vpn-letsdefend.io`) |
| **Result** | True Positive |

---

## 🔄 Incident Response Phases (NIST/SANS Framework)

### Phase 1: Preparation & Detection
* **Alert Trigger:** Detection rule flagged an incoming VPN authentication connection attempt targeting `https://vpn-letsdefend.io` originating from an unauthorized geographic location/IP (`113.161.158.12`).
* **Initial Observation:** External connection attempt targeted user account `monica@letsdefend.io`.

### Phase 2: Identification & Analysis
* **Artifact & Indicator Triage (IoCs):**
  * **User Account:** `monica@letsdefend.io`
  * **Source IP Address:** `113.161.158.12` (Unauthorized Country / Anomaly)
  * **Destination IP / Service:** `33.33.33.33` (`https://vpn-letsdefend.io`)
* **Authentication Log Forensics:**
  * Log analysis revealed that primary credentials (username and password) were **successfully matched** by the actor.
  * Subsequent Multi-Factor Authentication (MFA) failed due to repeated `"Incorrect OTP Code"` errors entered approximately one minute apart.
* **Verdict Distinction:** 
  * The alert is classified as a **True Positive** for an unauthorized **VPN connection attempt** from an anomalous location.
  * Although the actor did not achieve a successful session login (due to MFA protection), valid primary credentials were exposed and actively used from an untrusted source IP.

### Phase 3: Containment, Eradication & Recovery
* **Containment:**
  * **Credential Invalidation:** Immediately reset primary credentials and revoked all active sessions for `monica@letsdefend.io` to neutralize compromised password risk.
  * **IP Access Block:** Blacklisted IP `113.161.158.12` at the perimeter firewall and VPN gateway levels.
* **Eradication:**
  * **MFA Token Reset:** Re-registered and verified OTP tokens/devices bound to user `monica@letsdefend.io`.
* **Recovery:**
  * **User Contact & Verification:** Contacted user Monica to verify baseline travel status, confirm credential reset, and monitor account activity post-recovery.

### Phase 4: Post-Incident Activity & Lessons Learned
* **MFA Validation Value:** Multi-Factor Authentication successfully mitigated a potential breach despite compromised primary user credentials.
* **Credential Hygiene:** Primary password for `monica@letsdefend.io` was compromised prior to this connection attempt (likely via credential stuffing or password re-use). Mandatory dark web / breach monitoring should be implemented.
* **Geo-Fencing Policies:** Tightened conditional access policies to block VPN connection attempts from non-whitelisted countries before reaching the password prompt phase.

---

## 🎯 MITRE ATT&CK Mapping

* **T1133 - External Remote Services:** Exploitation or connection attempt to external enterprise VPN (`https://vpn-letsdefend.io`)
* **T1595 - Active Scanning:** IP probing / connection testing against external service endpoints
* **T1621 - Multi-Factor Authentication Request:** Failed OTP authentication attempts following successful primary credential entry
