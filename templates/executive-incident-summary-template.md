# Executive Incident Summary Template

---

### Purpose

This template is designed to provide a concise, high-level overview of a significant security incident for executive leadership and non-technical stakeholders. It focuses on the business impact, status, and strategic next steps, rather than deep technical details. The goal is to enable rapid understanding and decision-making at the executive level.

### Key Information Sections

#### 1. Incident Overview

-   **Incident Title:** A clear, concise title summarizing the incident (e.g., "Ransomware Attack on Production Servers," "Data Exfiltration Event," "Major Phishing Campaign").
-   **Date/Time Detected:** When the incident was first identified.
-   **Date/Time Initial Impact:** When the incident began to affect operations or data.
-   **Current Status:** (e.g., "Contained," "Eradicated," "Recovering," "Monitoring").
-   **Affected Systems/Areas:** High-level description of what was impacted (e.g., "Production Database," "Corporate Network," "Customer Data").
-   **Threat Type:** (e.g., "Ransomware," "Data Breach," "Phishing," "Insider Threat").

#### 2. Business Impact Assessment

-   **Severity Level:** (e.g., "Critical," "High," "Medium," "Low").
-   **Key Business Impacts:** Summarize the direct and indirect impact on business operations.
    -   Financial (e.g., "Estimated revenue loss: $X," "Recovery costs: $Y").
    -   Operational (e.g., "Customer-facing application offline for Z hours," "Disruption to supply chain").
    -   Reputational (e.g., "Potential negative media coverage," "Loss of customer trust").
    -   Regulatory/Legal (e.g., "Potential GDPR/HIPAA violation," "Notification requirements").
-   **Data Impact (if applicable):**
    -   Type of data affected (e.g., "Customer PII," "Proprietary source code," "Financial records").
    -   Volume/Scope of data affected.
    -   Confirmation of exfiltration (Yes/No/Under Investigation).

#### 3. Strategic Response & Resolution

-   **High-Level Actions Taken:** A brief summary of what the incident response team has done.
    -   "Containment measures implemented."
    -   "Impacted systems isolated and forensic images taken."
    -   "Customer notification process initiated."
    -   "Third-party forensics firm engaged."
-   **Current Mitigation/Containment:** What is currently in place to stop further damage or spread.
-   **Recovery Progress:** How far along are we in restoring normal operations?
    -   "X% of affected systems restored."
    -   "Target completion for full recovery: [Date/Time]."
-   **Key Decisions Needed (if any):** Clearly state any decisions that require executive input or approval.

#### 4. Post-Incident Outlook & Next Steps

-   **Lessons Learned (Preliminary):** Initial high-level observations that may influence future security posture.
-   **Long-Term Remediation/Hardening:** Strategic initiatives planned to prevent recurrence or reduce impact of similar incidents (e.g., "Accelerate MFA rollout," "Implement advanced EDR on all servers").
-   **Communication Plan:** Outline internal and external communication strategies (e.g., "Next update to board at X time," "Press release scheduled").
-   **Point of Contact:** Primary person for executive inquiries.

### Example Template Structure (Markdown)

```markdown
# EXECUTIVE INCIDENT SUMMARY: [INCIDENT TITLE]

**Date/Time of Report:** 2026-01-29 16:00 UTC
**Prepared By:** Incident Response Team

---

## 1. Incident Overview

-   **Incident Title:** Ransomware Attack Impacting Production Database
-   **Date/Time Detected:** 2026-01-29 08:30 UTC
-   **Date/Time Initial Impact:** 2026-01-29 07:00 UTC
-   **Current Status:** Contained, Recovery Initiated
-   **Affected Systems/Areas:** Primary Production Database Cluster, several web servers
-   **Threat Type:** Ransomware (identified as `[Ransomware Family]`)

## 2. Business Impact Assessment

-   **Severity Level:** Critical
-   **Key Business Impacts:**
    -   **Financial:** Estimated direct recovery cost $50,000. Potential revenue loss of $250,000 due to application downtime.
    -   **Operational:** Main customer-facing application offline for 9 hours. Internal reporting systems impacted.
    -   **Reputational:** High risk of negative media attention. Customer trust may be eroded.
    -   **Regulatory/Legal:** Under investigation for potential PII exposure. Initial assessment suggests no PII exfiltration, but forensic analysis ongoing.
-   **Data Impact:** Production customer database encrypted. No confirmed exfiltration of data, but cannot be ruled out until forensics complete.

## 3. Strategic Response & Resolution

-   **High-Level Actions Taken:**
    -   Isolated affected segments of production network.
    -   Engaged third-party ransomware negotiation and forensics experts.
    -   Initiated recovery from last known good backups.
    -   Notified relevant regulatory bodies (pending confirmation of PII).
-   **Current Mitigation/Containment:** Network segmentation enforced. All known IOCs blocked. Vulnerability patched.
-   **Recovery Progress:** Database restoration is 40% complete. Target full recovery by 2026-01-30 08:00 UTC.
-   **Key Decisions Needed:** Approval to procure additional cloud resources for expedited recovery (estimated $10,000).

## 4. Post-Incident Outlook & Next Steps

-   **Lessons Learned (Preliminary):** Identified critical gaps in patching cycles and network segmentation.
-   **Long-Term Remediation/Hardening:** Expedite implementation of advanced endpoint protection, enhance backup verification, conduct comprehensive penetration test of production environment.
-   **Communication Plan:** Internal briefing to all department heads 2026-01-29 17:00 UTC. External customer statement drafted, pending legal review.
-   **Point of Contact:** [Name], Head of Security Operations
```
