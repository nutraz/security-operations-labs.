# Evidence Handling Guide

---

### 1. Introduction & Importance

Digital evidence is information of probative value that is stored or transmitted in binary form. In the context of cybersecurity, this often includes logs, network traffic, disk images, memory dumps, and configuration files. Proper handling of digital evidence is paramount for ensuring its admissibility in legal proceedings, maintaining its integrity, and enabling accurate incident analysis.

This guide outlines the principles and procedures for collecting, preserving, analyzing, and presenting digital evidence in a forensically sound manner.

### 2. Principles of Digital Evidence

The fundamental principles guiding digital evidence handling are often summarized as:

-   **Admissibility:** Evidence must be legally obtained and relevant to the case.
-   **Authenticity:** The evidence must be proven to be what it purports to be.
-   **Completeness:** All relevant evidence should be collected and preserved.
-   **Reliability:** The evidence must be collected and handled in a manner that maintains its integrity and accuracy.
-   **Credibility:** The methods used to obtain and analyze the evidence must be generally accepted and applied consistently.

### 3. Phases of Evidence Handling

#### 3.1. Collection

The process of identifying, labeling, recording, and acquiring data from the most volatile to the least volatile sources.

-   **Prioritization (Order of Volatility):** Collect volatile data first (CPU cache, RAM, network connections) before non-volatile data (hard drives, logs).
-   **Identification:** Pinpoint sources of potential evidence (affected systems, network devices, cloud services, user accounts).
-   **Documentation:** Record everything:
    -   Date and time of collection.
    -   Who performed the collection.
    -   Tools and methods used (including version numbers).
    -   Location of the original evidence.
    -   Hashing of original data (if possible) and collected images.
    -   Photographs of the scene/system if relevant.
-   **Acquisition:** Use forensically sound methods to create bit-for-bit copies (images) of data sources.
    -   **Memory:** Tools like Magnet RAM Capture, FTK Imager Lite, `dumpit.exe`.
    -   **Disk:** Hardware write-blockers, FTK Imager, `dd`, `dcfldd`.
    -   **Logs:** Securely export logs from SIEM, servers, firewalls.
    -   **Network:** Packet captures (PCAP) from network taps orSPAN ports.

#### 3.2. Preservation

Maintaining the integrity and authenticity of collected evidence to ensure it remains unchanged from the time of collection.

-   **Write-Protection:** Store all forensic images and copies on write-protected media.
-   **Hashing:** Generate cryptographic hashes (MD5, SHA1, SHA256) of all acquired evidence at the point of acquisition and periodically thereafter to detect any unauthorized modifications.
-   **Secure Storage:** Store evidence in a physically secure location with limited access (e.g., locked cabinet, secure forensic workstation).
-   **Chain of Custody:** Meticulously document every transfer, access, or modification of the evidence. (See `compliance/chain-of-custody-procedures.md` - *upcoming*)

#### 3.3. Analysis

The process of examining collected evidence to extract relevant information, identify patterns, and reconstruct events.

-   **Working Copies:** Always perform analysis on working copies of the evidence, never the original forensic image.
-   **Specialized Tools:** Utilize forensic workstations and specialized software.
    -   **Disk Analysis:** Autopsy, FTK Forensic Toolkit, X-Ways Forensics.
    -   **Memory Analysis:** Volatility 3.
    -   **Log Analysis:** SIEM platforms, custom scripts.
    -   **Network Analysis:** Wireshark, NetworkMiner.
-   **Methodical Approach:** Follow a structured methodology to ensure no relevant evidence is overlooked.
-   **Documentation:** Record all analysis steps, tools used, findings, and observations.

#### 3.4. Presentation

The process of summarizing and explaining the findings of the analysis to relevant stakeholders, which may include management, legal counsel, or law enforcement.

-   **Clarity:** Present findings in a clear, concise, and understandable manner, tailored to the audience.
-   **Objectivity:** Present facts and evidence-based conclusions, avoiding speculation.
-   **Support:** Ensure all conclusions are supported by documented evidence.
-   **Expert Testimony:** In legal contexts, expert witnesses may be required to testify on the methods used and the findings.

### 4. Tools & Techniques

-   **Hardware Write Blockers:** Physically prevent data modification during disk acquisition.
-   **Forensic Workstations:** Isolated, secure computing environments for evidence analysis.
-   **Hashing Utilities:** `hashdeep`, `certutil` (Windows), `sha256sum` (Linux).
-   **Imaging Tools:** FTK Imager, `dd`, `dcfldd`.
-   **Data Carving:** Recovering deleted files from unallocated space.
-   **String Search:** Searching for keywords or patterns within raw data.

### 5. Documentation & Chain of Custody

Comprehensive documentation is integral to every phase of evidence handling. It establishes the **Chain of Custody**, which is a chronological record of who has had physical possession of the evidence, when, and for what purpose. This ensures the integrity and reliability of the evidence.

---
*Proper evidence handling is a cornerstone of digital forensics and incident response, ensuring that findings are credible and actionable.*
