# SOC Metrics Documentation

---

### 1. Introduction & Importance of SOC Metrics

Security Operations Center (SOC) metrics are quantifiable measures used to evaluate the effectiveness, efficiency, and overall performance of a SOC. They provide insights into the SOC's ability to prevent, detect, and respond to cyber threats, enabling continuous improvement and demonstrating value to the organization.

**Importance:**
-   **Performance Measurement:** Assess how well the SOC is meeting its objectives.
-   **Resource Optimization:** Identify areas where resources (staff, tools) can be better allocated.
-   **Decision Making:** Provide data-driven insights for strategic planning and investment in security technologies.
-   **Accountability:** Demonstrate the SOC's contribution to the organization's security posture.
-   **Continuous Improvement:** Highlight weaknesses and strengths, driving efforts to enhance processes and capabilities.

### 2. Categories of SOC Metrics

SOC metrics can typically be grouped into several categories:

-   **Operational Metrics:** Focus on the day-to-day activities and efficiency of the SOC.
-   **Performance Metrics:** Measure the effectiveness of the SOC in achieving its security goals.
-   **Business Metrics:** Relate security performance to overall business objectives and risk.
-   **Capacity Metrics:** Assess the SOC's ability to handle workload.

### 3. Examples of Key Metrics

#### 3.1. Alert & Triage Metrics

-   **Number of Alerts Generated:** Total volume of alerts from all security tools.
-   **Number of Incidents Detected:** How many alerts escalated to confirmed incidents.
-   **False Positive Rate:** Percentage of alerts that are determined to be benign or non-malicious.
    -   `False Positive Rate = (Number of False Positives / Total Number of Alerts) * 100`
-   **True Positive Rate (Detection Rate):** Percentage of actual threats successfully detected.
    -   `True Positive Rate = (Number of True Positives / Total Number of Threats) * 100`
-   **Average Alert Triage Time:** Time from alert generation to initial assessment.

#### 3.2. Incident Response Metrics

-   **Mean Time to Detect (MTTD):** Average time taken to identify a security incident from its occurrence. (See `soc-documentation/kpi-calculation-examples.md` and `soc-documentation/mttd-mttr-tracking-guide.md` - *upcoming*)
-   **Mean Time to Respond (MTTR):** Average time taken to contain and resolve a security incident from detection. (See `soc-documentation/kpi-calculation-examples.md` and `soc-documentation/mttd-mttr-tracking-guide.md` - *upcoming*)
-   **Incident Volume:** Total number of incidents handled over a period.
-   **Incident Severity Distribution:** Breakdown of incidents by severity (e.g., Critical, High, Medium, Low).
-   **Containment Time:** Time taken to contain an incident after detection.
-   **Eradication Time:** Time taken to eliminate the threat from the environment.
-   **Recovery Time:** Time taken to restore affected systems and services to normal operation.

#### 3.3. Threat Intelligence Metrics

-   **Number of IOCs Blocked:** How many malicious indicators were successfully blocked by security controls.
-   **Threat Intelligence Feed Effectiveness:** How often does ingested threat intelligence contribute to detections.
-   **Threat Hunting Detections:** Number of incidents discovered through proactive threat hunting.

#### 3.4. Vulnerability Management Metrics

-   **Average Patch Cycle Time:** Time taken to apply patches to systems after release.
-   **Number of Critical Vulnerabilities:** Count of open critical vulnerabilities in the environment.
-   **Vulnerability Remediation Rate:** Percentage of identified vulnerabilities patched within a service level agreement (SLA).

### 4. How to Collect and Report Metrics

-   **Centralized Data Sources:** Leverage SIEM, EDR, vulnerability scanners, and incident management systems as primary data sources.
-   **Automation:** Automate data collection and calculation where possible to ensure consistency and efficiency.
-   **Dashboards & Reporting Tools:** Use tools (e.g., SIEM dashboards, BI tools) to visualize metrics and create regular reports for different audiences (e.g., daily SOC team, weekly management, monthly executive).
-   **Contextualization:** Always present metrics with context. Explain what a number means and why it's important.

### 5. Linking Metrics to Security Goals

Metrics should not be collected in isolation; they must align with the organization's overarching security goals and risk appetite.

-   **Example Goal:** "Reduce the risk of successful ransomware attacks."
    -   **Relevant Metrics:** MTTD for ransomware, MTTR for ransomware, successful ransomware prevention rate, number of critical vulnerabilities on critical assets.
-   **Example Goal:** "Improve detection of advanced persistent threats."
    -   **Relevant Metrics:** True positive rate, threat hunting detection rate, threat intelligence feed effectiveness.

---
*Regular review and adjustment of metrics are crucial to ensure they remain relevant and continue to drive meaningful improvements in SOC operations.*
