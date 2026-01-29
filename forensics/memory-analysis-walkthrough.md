# Memory Analysis Walkthrough: Investigating a PowerShell Payload

---

### 1. Introduction & Use Cases

Memory forensics is the analysis of a computer's volatile memory (RAM) dump. It is a critical technique in incident response for uncovering evidence that may not exist on the hard drive, especially in cases of fileless malware, credential theft, and advanced persistence techniques.

**Common Use Cases:**
-   **Detecting Fileless Malware:** Identify malware that exists only in memory to evade disk-based detection.
-   **Analyzing Malware Behavior:** Observe a malware's loaded modules, network connections, and injected code.
-   **Recovering Credentials:** Extract plaintext passwords, password hashes, and encryption keys from memory (e.g., from the LSASS process).
-   **Investigating Rootkits:** Uncover hidden processes, files, and network connections.
-   **Timeline Reconstruction:** Identify running processes and user activity at the time of acquisition.

### 2. Tools Overview

-   **Volatility 3:** The industry-standard framework for memory analysis. It provides a suite of plugins for extracting artifacts from memory dumps. We will use Volatility 3 in this walkthrough.
-   **WinDBG:** A powerful debugger from Microsoft that can be used for live memory analysis and kernel debugging.

### 3. Memory Acquisition

Acquiring a memory dump is the first step. The method depends on the environment and available tools.

-   **EDR/Forensics Tools:** Most commercial EDR solutions (e.g., CrowdStrike Falcon, SentinelOne) and forensics suites (e.g., FTK Imager, Magnet RAM Capture) have built-in capabilities to remotely acquire a memory dump from an endpoint. This is the preferred method in most scenarios.
-   **Live System Tools:** On a live system, tools like `dumpit.exe` or `winpmem.exe` can be used.

For this walkthrough, we will assume a memory dump named `WS-FINANCE-05-20260129.mem` has been acquired and is ready for analysis.

### 4. Analysis Scenario

We are investigating an alert from our SIEM indicating suspicious PowerShell activity on workstation `WS-FINANCE-05`. The alert references a potential PowerShell download cradle, similar to the activity described in `T1059.001_PowerShell.md`.

Our goal is to analyze the memory dump from the workstation to determine if a malicious payload was executed and what its capabilities are.

### 5. Common Plugins & Commands (Volatility 3)

Let's begin our analysis using Volatility 3.

**Command Syntax:** `python vol.py -f <memory_dump_file> <plugin>`

#### 5.1. Identify the OS and Profile

First, we need to identify the correct operating system profile.

```bash
python vol.py -f WS-FINANCE-05-20260129.mem windows.info
```

**Output:**
```
Volatility 3 Framework 2.0.0
Kernel Base     0xf800043e1000
DTB             0x1ad000
Symbols         file:///<path_to_symbols>/windows_10_x64_19041.json
Is 64-bit       True
...
```
*The framework has automatically identified the Windows 10 profile.*

#### 5.2. List Running Processes (`windows.pslist`)

Let's list the processes running at the time of the memory dump to get a baseline.

```bash
python vol.py -f WS-FINANCE-05-20260129.mem windows.pslist
```

**Output (Abbreviated):**
```
PID   PPID  ImageFileName      Offset(V)            Threads   Handles   SessionId   Wow64
...
4     0     System             0xdeadbeefdeadbeef   150       -         -           False
...
620   748   svchost.exe        0xdeadbeefdeadbeef   12        -         0           False
3424  868   OUTLOOK.EXE        0xdeadbeefdeadbeef   45        -         1           False
4012  3424  powershell.exe     0xdeadbeefdeadbeef   5         -         1           False
4288  4012  rundll32.exe       0xdeadbeefdeadbeef   2         -         1           False
...
```

**Analysis:**
- We immediately spot `powershell.exe` (PID 4012) with a parent process of `OUTLOOK.EXE` (PID 3424). This is highly suspicious, as a user opening a malicious attachment or link in an email is a common infection vector.
- We also see that `powershell.exe` has spawned `rundll32.exe` (PID 4288), another red flag often associated with loading malicious DLLs.

#### 5.3. Check Network Connections (`windows.netscan`)

Let's check for active network connections, mapping them to the processes we just identified.

```bash
python vol.py -f WS-FINANCE-05-20260129.mem windows.netscan
```

**Output (Abbreviated):**
```
Offset      Proto  LocalAddr      LocalPort  ForeignAddr       ForeignPort  State    PID   Owner         Created
...
0xdeadbeef  TCP    192.168.1.55   49782      45.67.89.123      80           ESTABLISHED  4288  rundll32.exe  2026-01-29 14:22:10Z
...
```

**Analysis:**
- The suspicious `rundll32.exe` process (PID 4288) has an established connection to `45.67.89.123` on port 80. This IP was previously identified as a malicious C2 server in our phishing walkthrough.
- This is a strong **Indicator of Compromise (IOC)**.

#### 5.4. Find Injected Code (`windows.malfind`)

The `malfind` plugin is a powerful tool for finding injected code and other suspicious memory regions within a process.

```bash
python vol.py -f WS-FINANCE-05-20260129.mem windows.malfind -p 4288
```

**Output (Abbreviated):**
```
Process: rundll32.exe Pid: 4288
Address      Protection     Protection      Type      Size  Hexdump
0x000000e58d200000 PAGE_EXECUTE_READWRITE ---       RWX       Private   65536
00000000  4d 5a 90 00 03 00 00 00 04 00 00 00 ff ff 00 00  MZ..............
00000010  b8 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00  ........@.......
...
```

**Analysis:**
- `malfind` has identified a memory region in `rundll32.exe` with `PAGE_EXECUTE_READWRITE` (RWX) permissions. This is a classic sign of injected code. Legitimate processes rarely have memory segments that are simultaneously readable, writable, and executable.
- The hexdump shows the `MZ` header, indicating a Windows executable (likely a DLL) has been injected directly into the memory of `rundll32.exe`. This is a form of **Process Injection (T1055)**.

We can dump this memory region for further analysis:
`python vol.py -f WS-FINANCE-05-20260129.mem windows.dumpfiles --virtaddr 0x000000e58d200000 -p 4288`

### 6. Detection Patterns & MITRE Mapping

Based on our analysis, we have identified several key patterns:

-   **Suspicious Parent-Child Process Relationship:** `OUTLOOK.EXE -> powershell.exe -> rundll32.exe`.
-   **Network Traffic to Known Bad IP:** Connection to `45.67.89.123`.
-   **Injected Code in Memory:** An executable (`MZ`) found in a `RWX` memory section of `rundll32.exe`.

**MITRE ATT&CK Techniques:**
-   **T1059.001:** PowerShell (Initial execution via Outlook).
-   **T1055:** Process Injection (Injecting code into `rundll32.exe`).
-   **T1003.001:** LSASS Memory (The next step would be to dump LSASS memory to check for credential theft).

### 7. Integration with EDR/SIEM

This memory analysis confirms the initial alert and provides critical context that might be missed by EDR alone.

-   **EDR:** The IOCs (file hashes of the dumped malware, C2 IP address) should be added to the EDR for blocking and wider threat hunting. See `edr_response_workflow.md`.
-   **SIEM:** The findings enrich the original alert. The C2 IP can be used to search for other compromised hosts in network logs.
-   **Incident Response:** This analysis is a key part of the investigation phase of our ransomware playbook (`playbook_ransomware.md`).

---
*This walkthrough provides a foundational example. Real-world analysis often involves many more steps and plugins to build a complete picture of the compromise.*
