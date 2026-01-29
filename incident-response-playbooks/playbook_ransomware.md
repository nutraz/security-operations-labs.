# Incident Response Playbook: Ransomware

This document outlines the high-level workflow for responding to a ransomware incident.

-   **Version:** 1.0
-   **Last Updated:** 2026-01-28
-   **Owner:** SOC Team

---

### 1. Preparation

-   Ensure backups are regularly tested and stored offline/off-site.
-   Ensure network segmentation is in place to limit lateral movement.
-   Deploy and configure EDR/AV solutions with anti-ransomware capabilities.
-   Have contact information for legal, management, and cyber insurance readily available.

### 2. Identification

-   **Initial Indicators:**
    -   EDR/AV alerts for ransomware activity.
    -   Users reporting files they cannot open.
    -   Ransom notes found on desktops or in directories.
    -   Rapid, high-volume file modifications on file shares.
    -   System running unusually slow.
-   **Action:**
    -   **CONFIRM** the activity is ransomware.
    -   **DO NOT** delete the ransom note; it may contain information needed for decryption if that path is chosen.
    -   Declare a major incident and activate the IR team.

### 3. Containment

-   **IMMEDIATE GOAL:** Stop the bleeding. Prevent the ransomware from spreading further.
-   **Actions:**
    -   **Isolate affected hosts:** Disconnect them from the network immediately (unplug ethernet cable, disable Wi-Fi). Do NOT power them off unless instructed, as this destroys volatile memory evidence.
    *   **Decision Point: Power Off or Keep Running?**
        *   **IF** Memory forensics is required (e.g., fileless malware, advanced persistent threat suspected): Keep system running, acquire memory dump before isolation.
        *   **ELSE IF** Rapid containment is paramount, and memory forensics is not a priority (e.g., clear, widespread file encryption): Power off immediately after documenting state.
    -   **Segment the network:** If possible, use VLANs or firewall rules to isolate the subnet where the infection is located.
    -   **Disable compromised user accounts:** The initial entry vector may be a compromised account. Disable it pending investigation.
    -   **Block C2 servers:** If any Command & Control IP addresses or domains are identified, block them at the firewall/proxy.

### 4. Eradication

-   **GOAL:** Remove the adversary and their tools from the environment.
-   **Actions:**
    -   Identify the initial point of entry (e.g., phishing email, vulnerable public-facing server).
    -   Identify all compromised systems and user accounts.
    -   For affected systems, the safest path is to **rebuild from a known-good, clean image**. Do not simply "clean" the machine, as rootkits or backdoors may persist.
    *   **Decision Point: Rebuild or Attempt Clean?**
        *   **IF** System is critical and rebuild time is prohibitive, AND confidence in cleaning tools is high: Attempt clean with multiple reputable tools. Proceed with extreme caution and continuous monitoring.
        *   **ELSE** (Recommended) Rebuild the system from scratch using a trusted, hardened image.
    -   Force a password reset for all users, especially for compromised and administrative accounts.

### 5. Recovery

-   **GOAL:** Restore normal business operations.
-   **Actions:**
    -   Restore encrypted files from clean, tested backups. Prioritize critical systems first.
    *   **Decision Point: Pay Ransom or Restore from Backup?**
        *   **IF** No viable/recent backups exist OR data criticality demands immediate decryption AND organizational policy permits: Engage specialized ransomware negotiation firm.
        *   **ELSE** (Recommended) Do NOT pay the ransom. Restore from verified clean backups.
    -   Bring rebuilt systems back online, ensuring they are fully patched and configured with enhanced security settings.
    -   Monitor the environment closely for any signs of residual adversary activity.

### 6. Post-Incident Activities (Lessons Learned)

-   Conduct a root cause analysis (RCA).
-   Update security controls and policies to prevent re-infection.
-   Update this playbook with any new information or improved procedures.
-   Provide a final report to management.
