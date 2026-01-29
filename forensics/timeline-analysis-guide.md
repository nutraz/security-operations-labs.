# Timeline Analysis Guide

---

### 1. Introduction & Importance

Timeline analysis is the process of reconstructing a sequence of events from various forensic artifacts to understand the "who, what, when, where, and how" of an incident. It helps investigators correlate disparate pieces of evidence, identify attack patterns, and determine the exact chronology of malicious activities.

**Importance in Incident Response:**
-   **Event Correlation:** Connects events from different sources (logs, disk, memory) into a single, coherent narrative.
-   **Attacker Activity Mapping:** Provides a chronological view of an attacker's actions, from initial access to data exfiltration.
-   **Scope of Compromise:** Helps determine the duration and extent of an incident.
-   **Root Cause Analysis:** Pinpoints the initial entry point and vulnerabilities exploited.

### 2. Data Sources for Timeline Construction

A robust timeline integrates data from multiple sources for a comprehensive view.

-   **System Logs:**
    -   **Windows Event Logs (`.evtx`):** Security, System, Application, PowerShell, Sysmon (if deployed).
    -   **Linux Logs (`/var/log`):** `auth.log`, `syslog`, Apache/Nginx access logs.
-   **Disk Forensics Artifacts:** (Refer to `forensics/disk-forensics-methodology.md`)
    -   **MFT (Master File Table):** File creation, modification, access, and entry modification timestamps.
    -   **Registry Hives:** Last written times for keys and values, user activity (UserAssist).
    -   **Link Files (`.lnk`):** Creation, modification, and access times of shortcuts, often pointing to external drives.
    -   **Prefetch Files (`.pf`):** Last run times of executables.
    -   **AppCompatCache/ShimCache:** Records of executed programs.
    -   **Browser History:** Timestamps of visited websites and downloads.
-   **Network Logs:**
    -   **Firewall/Proxy Logs:** Connection attempts, allowed/denied traffic, destination IPs/domains.
    -   **DNS Logs:** DNS queries made by compromised hosts.
    -   **Packet Captures (PCAPs):** Detailed network communications.
-   **Memory Forensics Artifacts:** (Refer to `forensics/memory-analysis-walkthrough.md`)
    -   Process creation/termination times.
    -   Network connection establishment times.

### 3. Tools for Timeline Analysis

-   **log2timeline/plaso:** A Python-based framework for generating super timelines. It can parse a wide variety of forensic artifacts into a unified format (`.plaso` file), which can then be exported to CSV, Excel, or other formats for analysis.
    ```bash
    # Example: Generating a plaso file from a disk image
    log2timeline.py --preprocess --partition p1 -o my_case.plaso image.dd

    # Example: Listing events from a plaso file
    psort.py -o l2tcsv -w events.csv my_case.plaso "date > '2026-01-20 00:00:00'"
    ```
-   **Timeline Explorer (Eric Zimmerman):** A GUI tool for viewing and filtering super timelines (e.g., from plaso output or directly from various Windows artifacts).
-   **grep/awk/sed (Linux/Unix):** Command-line tools for parsing and filtering text-based logs.
-   **SIEM Platforms (Splunk, Sentinel):** Can ingest and correlate various log sources to build timelines.

### 4. Methodology for Timeline Construction

**4.1. Data Collection:**
-   Acquire disk images, memory dumps, and export relevant logs from affected systems and network devices.
-   Ensure proper chain of custody for all collected evidence.

**4.2. Parsing & Normalization:**
-   Use tools like `log2timeline.py` to parse raw artifacts and extract timestamped events.
-   Normalize timestamps to Coordinated Universal Time (UTC) to avoid timezone issues.

**4.3. Filtering & Correlation:**
-   Focus on a specific timeframe relevant to the incident.
-   Filter events based on keywords, process names, IP addresses, or user accounts.
-   Correlate events across different data sources (e.g., a process execution in an event log, followed by a network connection in a firewall log, and code injection identified via memory analysis).

**4.4. Visualization & Reporting:**
-   Represent the timeline visually (e.g., in a spreadsheet, specialized timeline tool, or SIEM dashboard) to identify patterns and anomalies.
-   Document the narrative of the incident, supported by chronological evidence.

### 5. Key Artifacts for Timeline Correlation

-   **Process Execution:** `Sysmon Event ID 1`, `4688` (Windows), `audit.log` (Linux).
-   **Network Connections:** Firewall logs, proxy logs, `Sysmon Event ID 3`, `netscan` from Volatility.
-   **File System Activity:** MFT records, `Sysmon Event ID 11` (FileCreate).
-   **User Logons:** `Event ID 4624/4625` (Windows), `auth.log` (Linux).
-   **Scheduled Tasks:** Registry `Task Scheduler` keys, `schtasks.exe` output.

### 6. Integration with Other Forensics Areas

-   **Memory Analysis:** Timeline analysis helps contextualize findings from memory dumps (e.g., when a suspicious process started, when a network connection was established).
-   **Disk Forensics:** Provides the raw data (MFT, Registry, logs) that forms the backbone of any detailed timeline. The artifacts recovered from disk are crucial for fleshing out event details.

---
*Timeline analysis is an iterative process. Initial timelines often raise new questions that require deeper dives into specific artifacts or additional data collection.*
