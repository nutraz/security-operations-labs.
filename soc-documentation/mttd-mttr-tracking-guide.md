# MTTD/MTTR Tracking Guide

---

### 1. Introduction: Understanding MTTD and MTTR

Mean Time to Detect (MTTD) and Mean Time to Respond (MTTR) are two of the most critical Key Performance Indicators (KPIs) for any Security Operations Center (SOC). They directly measure the SOC's efficiency and effectiveness in identifying and neutralizing cyber threats.

-   **Mean Time to Detect (MTTD):** The average time taken for a security team to identify a security incident from the moment it began or occurred. A lower MTTD indicates faster detection capabilities.
-   **Mean Time to Respond (MTTR):** The average time taken for a security team to fully resolve a security incident once it has been detected. This includes containment, eradication, and recovery. A lower MTTR indicates faster incident resolution.

**Why are these metrics important?**
-   **Minimize Impact:** Faster detection and response reduce the window of opportunity for attackers, thereby minimizing the financial, operational, and reputational damage of an incident.
-   **Measure SOC Performance:** Provide clear, quantifiable data on the SOC's operational effectiveness.
-   **Resource Justification:** Help justify investments in tools, training, and personnel by demonstrating tangible improvements.
-   **Continuous Improvement:** Highlight areas for process and technology optimization.

### 2. Measuring the Phases of an Incident

To accurately track MTTD and MTTR, it's crucial to define and measure the time spent in each phase of the incident response lifecycle.

#### 2.1. Detection Phase (for MTTD)

-   **Start Time (Incident Occurrence):** The actual time the malicious activity began. This can be challenging to pinpoint precisely but often correlates with initial compromise, malware execution, or unusual user activity. Data sources include system logs, network flows, EDR telemetry.
-   **End Time (Incident Detected):** The time the SOC (or automated system) first identified the malicious activity and escalated it as a potential incident. This might be when an alert is triggered in the SIEM, or an analyst begins triage.
-   **MTTD Calculation:** `Time of Detection - Time of Occurrence`

#### 2.2. Response Phase (for MTTR)

The response phase can be broken down into sub-phases, each contributing to the overall MTTR.

-   **Containment:**
    -   **Start Time:** When the SOC begins actions to limit the scope and spread of the incident (e.g., isolating a host, blocking an IP).
    -   **End Time:** When the incident's spread is successfully halted, and immediate danger is mitigated.
    -   **Metric:** Mean Time to Contain (MTTC). `Time of Containment - Time of Detection`
-   **Eradication:**
    -   **Start Time:** When the SOC begins removing the threat from the environment (e.g., malware cleanup, account disablement, patching vulnerabilities).
    -   **End Time:** When the threat is confirmed to be completely removed.
    -   **Metric:** Mean Time to Eradicate (MTTE). `Time of Eradication - Time of Containment`
-   **Recovery:**
    -   **Start Time:** When systems and data are restored to normal operation (e.g., restoring from backup, rebuilding systems).
    -   **End Time:** When all affected systems and services are fully operational, verified, and secured.
    -   **Metric:** Mean Time to Recover (MTTR - specific to recovery). `Time of Recovery - Time of Eradication`

**Overall MTTR Calculation:** `Time of Recovery (full resolution) - Time of Detection`
*Alternatively, some definitions calculate MTTR as the sum of MTTC, MTTE, and MTTR (recovery).* Be consistent with your chosen definition.

### 3. Tools and Processes for Tracking

Accurate time tracking is essential and relies on robust processes and integrated tools.

-   **SIEM (Security Information and Event Management):**
    -   **Detection Time:** Automated alerts in SIEM can mark the start of detection.
    -   **Incident Occurrence:** Logs in SIEM can help pinpoint the initial activity.
-   **SOAR (Security Orchestration, Automation, and Response):**
    -   **Automated Time Stamping:** SOAR playbooks can automatically timestamp phase transitions (e.g., "incident created," "containment action initiated," "incident closed").
    -   **Workflow Enforcement:** Guides analysts through consistent incident response steps, ensuring key data points are captured.
-   **ITSM/Incident Management System (e.g., ServiceNow, Jira Service Management):**
    -   **Incident Lifecycle Management:** Provides a structured system for opening, assigning, tracking, and closing incidents.
    -   **Audit Trail:** Records all actions taken, including timestamps for each status change.
    -   **Custom Fields:** Can be used to manually input or verify incident start times, if not automatically derived.
-   **Forensic Timelines:** Generated during forensic analysis (e.g., using `log2timeline/plaso` as discussed in `forensics/timeline-analysis-guide.md`) can provide a precise "Time of Occurrence."
-   **Standard Operating Procedures (SOPs):** Clear SOPs guide analysts on how and when to update incident statuses in the tracking system, ensuring consistent data capture.

### 4. Strategies for Improving MTTD and MTTR

#### 4.1. Improving MTTD

-   **Enhance Detection Capabilities:**
    -   Improve threat intelligence integration (`threat-intelligence/threat-feed-integration-guide.md`).
    -   Develop more effective detection rules (e.g., KQL, Splunk, Sigma rules from `detection-rules/`).
    -   Reduce false positives through rule tuning and baselining.
    -   Invest in advanced detection technologies (e.g., behavioral analytics, AI/ML).
-   **Optimize Alert Triage:**
    -   Automate initial alert enrichment (`threat-intelligence/ioc-enrichment-workflow.md`).
    -   Prioritize alerts based on risk and context.
    -   Provide clear playbooks for alert investigation.
-   **Centralized Logging:** Ensure all relevant logs are ingested into the SIEM and easily searchable.

#### 4.2. Improving MTTR

-   **Automate Response Actions:** Implement SOAR playbooks for common incident types (e.g., automated host isolation, password resets).
-   **Develop Clear Incident Response Playbooks:** Provide step-by-step guidance for containment, eradication, and recovery for various incident types (`incident-response-playbooks/`).
-   **Regular Drills and Exercises:** Conduct tabletop exercises and simulations to practice incident response.
-   **Cross-Functional Collaboration:** Foster strong communication and collaboration with IT, legal, communications, and other relevant teams.
-   **Effective Tools:** Ensure analysts have access to effective forensic (e.g., `forensics/`) and response tools.
-   **Skill Development:** Invest in continuous training for SOC analysts.

---
*Consistent tracking and analysis of MTTD and MTTR are vital for maturing a SOC's capabilities and resilience against cyber threats.*
