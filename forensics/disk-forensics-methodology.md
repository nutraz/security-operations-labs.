# Disk Forensics Methodology

---

### 1. Introduction & Use Cases

Disk forensics involves the scientific examination of digital storage media (hard drives, SSDs, USB drives, etc.) to identify, preserve, recover, analyze, and present facts about digital information. It's crucial for understanding the full scope of a compromise, persistent malware, and user activity.

**Common Use Cases:**
-   **Malware Analysis:** Recovering malware executables, configuration files, and persistence mechanisms.
-   **Data Recovery:** Retrieving deleted files, partitions, or hidden data.
-   **User Activity Analysis:** Investigating browser history, recent documents, executed programs, and external device connections.
-   **Attribution & Timeline Construction:** Identifying when and how a system was compromised.
-   **Evidence Preservation:** Ensuring data integrity for legal or compliance purposes.

### 2. Acquisition: Creating a Forensic Image

The first and most critical step is to create a forensically sound image of the disk. This involves creating a bit-for-bit copy of the storage media without altering the original evidence.

**Key Principles:**
-   **Write-Blocking:** Always use a hardware or software write-blocker to prevent any modifications to the original disk.
-   **Hashing:** Generate cryptographic hashes (e.g., MD5, SHA1, SHA256) of the original drive and the forensic image immediately before and after acquisition to ensure integrity.
-   **Chain of Custody:** Document every step of the acquisition process, including who performed it, when, where, and what tools were used.

**Tools for Acquisition:**
-   **FTK Imager (AccessData):** Popular Windows-based tool for creating forensic images in various formats (DD, E01).
-   **Magnet RAM Capture (Magnet Forensics):** Can also acquire disk images.
-   **`dd` (Linux/Unix):** A command-line utility for raw disk imaging.
    ```bash
    # Example: dd if=/dev/sdX of=/mnt/forensic_drive/image.dd bs=4M conv=noerror,sync status=progress
    # Always ensure 'of' (output file) is not the original source disk!
    ```
-   **`dcfldd` (Linux/Unix):** Enhanced version of `dd` with hashing and status updates.

**Output Formats:**
-   **Raw (DD):** A bit-for-bit copy of the disk, typically with a `.dd` or `.img` extension.
-   **EnCase (E01):** Proprietary format that includes metadata, case information, and checksums. Widely used in forensics.

### 3. Tools for Analysis

Once an image is acquired, specialized tools are used for analysis.

-   **Autopsy (Open Source):** A comprehensive graphical interface for The Sleuth Kit (TSK), offering file system analysis, keyword searching, timeline generation, and more.
-   **FTK Forensic Toolkit (AccessData):** Commercial suite with advanced capabilities for disk analysis.
-   **X-Ways Forensics:** Another powerful commercial forensic analysis tool.
-   **Registry Explorer (Eric Zimmerman):** For detailed analysis of Windows Registry hives.
-   **Timeline Explorer (Eric Zimmerman):** For creating super-timelines from various artifacts.

### 4. Analysis Areas & Key Artifacts

**4.1. File System Analysis**
-   **NTFS/FAT/extX:** Understand the file system structure.
-   **Deleted Files:** Recover and examine files that have been deleted but not yet overwritten.
-   **Alternate Data Streams (ADS):** Hidden data streams in NTFS that can be used by malware for concealment.
-   **Timestamps (MACB):** Modified, Accessed, Created, and Entry Modified timestamps are crucial for timeline construction.

**4.2. Windows Registry Analysis**
The Registry is a goldmine for forensic information.
-   **Persistence:** `Run`, `RunOnce`, `Services`, `Scheduled Tasks` keys.
-   **User Activity:** `UserAssist`, `RecentDocs`, `OpenSaveMRU`.
-   **External Devices:** `USBSTOR` keys to identify connected USB devices.
-   **System Information:** OS version, installed software, network configurations.

**4.3. Event Logs (`.evtx`)**
-   **Security Log (Event ID 4624/4625):** Logon/logoff events, brute-force attempts.
-   **System Log:** System startup/shutdown, driver errors.
-   **Application Log:** Application-specific events.
-   **PowerShell Operational Log (Event ID 4104):** Critical for PowerShell activity.

**4.4. Prefetch Files (`.pf`)**
-   Windows Prefetch files record programs that have been executed on a system, including their execution count and last run time.
-   Helps identify what programs were run and how frequently.

**4.5. Link Files (`.lnk`)**
-   Shortcut files that contain metadata about the target file, including its path, timestamps, and network location if applicable.
-   Indicate user interaction with specific files.

**4.6. Browser History & Downloads**
-   Examine browser artifacts (Chrome, Firefox, Edge) to identify visited websites, download history, and search queries.

### 5. Integration Points

-   **Timeline Analysis:** Disk artifacts are essential for constructing a comprehensive timeline of events, often integrating with memory analysis and log data. (See `forensics/timeline-analysis-guide.md` - *upcoming*)
-   **Incident Response:** Disk forensics provides the detailed evidence needed for eradication and recovery phases in playbooks like `playbook_ransomware.md`.
-   **EDR/SIEM:** Findings from disk forensics can be used to create new detection rules and enhance threat hunting efforts.

---
*This methodology provides a general framework. Each investigation will have unique aspects requiring adaptation of tools and techniques.*
