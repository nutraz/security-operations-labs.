# Incident Response Playbook: Data Breach

This document outlines the high-level workflow for responding to a data breach incident.

-   **Version:** 1.0
-   **Last Updated:** 2026-01-29
-   **Owner:** SOC Team

---

### 1. Preparation

-   Implement robust data loss prevention (DLP) solutions.
-   Classify data based on sensitivity and apply appropriate access controls.
-   Maintain detailed logs for data access, modification, and transfer.
-   Establish clear communication protocols with legal, PR, and regulatory bodies.
-   Have data breach notification templates ready.

### 2. Identification

-   **Initial Indicators:**
    -   DLP alerts on sensitive data exfiltration.
    -   External notification (e.g., law enforcement, third-party, customer).
    -   Unusual database queries or large data transfers.
    -   Compromise of systems containing sensitive data.
    -   Discovery of company data on dark web or public repositories.
-   **Action:**
    -   **CONFIRM** the data breach and its scope.
    -   **IDENTIFY** the type and sensitivity of the compromised data.
    -   Engage legal counsel and privacy officer immediately.

### 3. Containment

-   **IMMEDIATE GOAL:** Stop further data loss and prevent ongoing unauthorized access.
-   **Actions:**
    -   **Isolate compromised systems/networks:** Disconnect systems where the breach occurred.
    -   **Revoke access:** Disable compromised accounts and revoke access tokens.
    -   **Block exfiltration channels:** Shut down any identified methods of data exfiltration (e.g., block external IPs, disable services).
    -   **Patch vulnerabilities:** If the breach was due to a vulnerability, apply patches or implement compensating controls.
    -   **Secure remaining data:** Ensure data not yet breached is secured with stronger controls.

### 4. Eradication

-   **GOAL:** Remove the threat actor and their persistence mechanisms from the environment.
-   **Actions:**
    -   **Identify root cause:** Determine how the breach occurred (e.g., vulnerability exploitation, compromised credentials).
    -   **Eliminate all adversary access:** Ensure all backdoors, malicious accounts, and persistence mechanisms are removed.
    -   **Sanitize affected systems:** Rebuild or thoroughly clean compromised systems.
    -   **Review logs:** Conduct a comprehensive review of logs to ensure no other systems were affected or persistence established.
    -   **Force password resets:** For all potentially compromised accounts.

### 5. Recovery

-   **GOAL:** Restore affected systems and data, and rebuild trust.
-   **Actions:**
    -   Restore data from secure backups if data was corrupted or deleted.
    -   Verify the integrity and availability of all systems and data.
    -   Implement enhanced monitoring for unusual data access or exfiltration.
    -   Notify affected individuals and regulatory bodies as legally required.
    -   Communicate transparently with stakeholders.

### 6. Post-Incident Activities (Lessons Learned)

-   Conduct a thorough forensic investigation and root cause analysis.
-   Update data handling policies, security controls, and incident response playbooks.
-   Review and improve employee training on data security and privacy.
-   Evaluate cyber insurance coverage and legal implications.
-   Provide a final report to management and external stakeholders.
