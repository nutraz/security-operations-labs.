# Alert Triage Example: Multiple Failed Logins (Brute Force)

### Alert Details

-   **Alert Name:** Excessive Failed Login Attempts
-   **Source IP:** `185.191.171.12`
-   **Target User:** `administrator`
-   **Target Host:** `WEBSRV01` (10.1.1.50)
-   **Timestamp:** 2026-01-28 03:15:00Z - 03:20:00Z
-   **Count:** 540 failed attempts in 5 minutes.

---

### Sample Logs (SIEM View)

```
Timestamp,EventID,Host,SourceIP,User,Status
...
2026-01-28T03:15:01Z,4625,WEBSRV01,185.191.171.12,administrator,Failure
2026-01-28T03:15:01Z,4625,WEBSRV01,185.191.171.12,administrator,Failure
2026-01-28T03:15:02Z,4625,WEBSRV01,185.191.171.12,administrator,Failure
...
(537 more similar events)
...
2026-01-28T03:20:15Z,4624,WEBSRV01,185.191.171.12,administrator,Success
```
*Note: Windows Security Event ID `4625` = An account failed to log on. Event ID `4624` = An account successfully logged on.*

### Triage & Analysis Walkthrough

**Step 1: Initial Assessment**

-   The alert indicates a high volume of failed logins against a critical account (`administrator`) on a web server.
-   The sudden appearance of a successful login (`EventID 4624`) immediately after a storm of failures is a **major red flag**. This suggests the brute force attack was successful.
-   **Initial Verdict:** High Priority. Potential host compromise.

**Step 2: External Intelligence (Who is the source?)**

-   Perform a Whois/reputation check on the source IP `185.191.171.12`.
    -   `whois 185.191.171.12`
    -   Check reputation on sources like AbuseIPDB, VirusTotal, etc.
-   **Finding:** The IP is associated with a known VPN/proxy service and has been reported for malicious activity multiple times. This is not legitimate traffic.

**Step 3: Internal Context (What was targeted?)**

-   `WEBSRV01` is a public-facing web server. What services is it running?
    -   Check asset inventory. Let's assume it's running RDP (Remote Desktop Protocol) open to the internet. This is a common vector for brute force attacks.
-   The `administrator` account is a high-privilege local admin account. A compromise would give the attacker full control of the host.

**Step 4: Correlate with Other Logs**

-   **Check EDR logs:** Look for any suspicious process execution on `WEBSRV01` immediately following the successful login at `03:20:15Z`.
    -   Did `svchost.exe` spawn `powershell.exe`?
    -   Are there any new network connections to unknown IPs?
-   **Check Firewall/VPN logs:** Has the attacker's IP been seen elsewhere on the network?

**Step 5: Verdict & Escalation**

-   **Verdict:** **True Positive.** This is a successful brute force attack against a public-facing server, leading to a host compromise.
-   **Immediate Actions:**
    1.  **Containment:** Isolate `WEBSRV01` from the network to prevent lateral movement.
    2.  **Eradication:** Disable the `administrator` account on the host.
    3.  **Escalation:** Escalate the incident to the Tier 2/Incident Response team, providing all findings.
    4.  **Blocking:** Block the attacker IP `185.191.171.12` at the network edge firewall.

### Follow-up Recommendations

-   Disable RDP on all public-facing servers or restrict it to known-good IPs via a bastion host/VPN.
-   Enforce a stronger account lockout policy.
-   Ensure the local `administrator` account has an extremely complex, long password and is renamed.
-   Review logs to determine the extent of post-compromise activity.
