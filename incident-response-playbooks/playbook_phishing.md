# Incident Response Playbook: Phishing

This document outlines the high-level workflow for responding to a phishing incident.

-   **Version:** 1.0
-   **Last Updated:** 2026-01-29
-   **Owner:** SOC Team

---

### 1. Preparation

-   Implement robust email filtering and anti-phishing solutions.
-   Conduct regular security awareness training for all employees on identifying phishing attempts.
-   Ensure email authentication mechanisms (SPF, DKIM, DMARC) are configured correctly.
-   Have clear reporting mechanisms for suspicious emails.

### 2. Identification

-   **Initial Indicators:**
    -   User reports suspicious email via reporting tool.
    -   Email gateway alerts on malicious content/links.
    -   Endpoint detection of malware downloaded via email.
    -   Credential harvesting attempts detected (e.g., failed logins after clicking a link).
-   **Action:**
    -   **CONFIRM** the email is malicious.
    -   **TRIAGE** the reported incident for severity and potential impact.
    -   Declare an incident if compromise is confirmed or highly likely.

### 3. Containment

-   **IMMEDIATE GOAL:** Prevent further compromise and spread.
-   **Actions:**
    -   **Remove malicious email:** Delete the identified phishing email from all user inboxes across the organization.
    -   **Block sender/domain/IP:** Add sender's email address, domain, or IP to block lists at the email gateway and firewall.
    -   **Block malicious URLs:** Add any malicious URLs to web proxy/firewall block lists.
    -   **Isolate compromised hosts:** If a user clicked a link or opened an attachment that led to compromise, isolate the affected system from the network.
    -   **Disable compromised user accounts:** If credentials were stolen, disable the account immediately.

### 4. Eradication

-   **GOAL:** Eliminate the root cause and any adversary presence.
-   **Actions:**
    -   **Analyze the phishing email:** Extract IOCs (Indicators of Compromise) such as sender, subject, URLs, attachment hashes.
    -   **Scan affected systems:** Run full antivirus/EDR scans on any potentially compromised endpoints.
    -   **Identify initial access vector:** Confirm how the phishing attempt was delivered and if it was successful.
    -   **Password reset:** Force a password reset for any accounts whose credentials may have been compromised.
    -   **Remove malware/backdoors:** Clean any detected malware or persistence mechanisms from compromised systems.

### 5. Recovery

-   **GOAL:** Restore affected systems and services to normal operation.
-   **Actions:**
    -   Re-enable user accounts after password resets and verification of no further compromise.
    -   Bring isolated systems back online after they have been thoroughly cleaned and validated.
    -   Verify that all malicious emails have been removed and block lists are effective.
    -   Communicate with affected users and provide guidance on future phishing prevention.

### 6. Post-Incident Activities (Lessons Learned)

-   Conduct a root cause analysis (RCA) – focusing on why the phishing email bypassed controls or why the user fell for it.
-   Update email filters, security awareness training, and incident response playbooks.
-   Share lessons learned with the wider security team and affected departments.
-   Provide a final report to management.
