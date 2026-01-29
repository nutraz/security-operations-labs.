# Stakeholder Update Format

---

### Purpose and Audience

This template provides a structured format for communicating ongoing security incident status to key stakeholders. These stakeholders typically include department heads, legal counsel, public relations, and potentially external partners. The goal is to keep them informed about the incident's progress, its impact, and the actions being taken, enabling them to manage their respective areas effectively.

The level of detail is typically higher than an executive summary but less technical than an internal incident response log.

### Key Sections

#### 1. Update Header

-   **Incident Title:** (Consistent with Executive Summary and other communications).
-   **Update Number:** (e.g., "Update #1," "Update #2").
-   **Date/Time of Update:** When this specific update is being issued.
-   **Reporting Period:** Covers the timeframe since the last update.
-   **Prepared By:** Incident Response Team.

#### 2. Incident Summary (Brief Refresh)

-   A very brief recap of the incident for context, especially for stakeholders who might not have read previous communications or are joining the loop.
    -   "Recapping: On [Date], we identified a [type of incident] affecting [affected systems/areas]."

#### 3. Current Status & Key Developments

-   **Overall Status:** (e.g., "Incident is contained and recovery is underway," "Investigation ongoing," "Threat remediated").
-   **Key Developments since Last Update:** Highlight significant changes or findings.
    -   "Forensic analysis confirms no data exfiltration."
    -   "The root cause has been identified as [specific vulnerability/action]."
    -   "All critical systems have been restored."
-   **Timeline Updates (if applicable):** Any changes to projected recovery times or milestones.

#### 4. Impact Assessment

-   **Current Business Impact:** Reiterate or update the current impact on operations, services, and reputation.
    -   "All customer-facing services are now fully operational."
    -   "Internal collaboration tools remain affected, with an estimated recovery by [Date]."
    -   "No evidence of PII compromise found to date."
-   **Financial Implications (if known):** Briefly mention any updated cost estimates for recovery or potential losses.
-   **Reputational Risks:** Current assessment of PR or public disclosure needs.

#### 5. Actions Taken & Mitigation Efforts

-   **Key Actions During Reporting Period:** Detail significant actions taken by the incident response team.
    -   "Containment strategy successfully implemented across all affected network segments."
    -   "Isolated and cleaned X number of affected endpoints."
    -   "Engaged [external vendor] for specialized assistance."
    -   "Implemented temporary workarounds for [affected service]."
-   **Mitigation Effectiveness:** Briefly comment on how effective the actions have been.

#### 6. Next Steps & Forward Plan

-   **Immediate Priorities:** What the IR team is focusing on in the next reporting period.
    -   "Continue with system restoration and validation."
    -   "Complete forensic analysis on remaining affected servers."
    -   "Implement long-term hardening measures."
-   **Expected Outcomes:** What stakeholders can anticipate by the next update.
-   **Dependencies/Needs:** Any support or decisions required from stakeholders.

#### 7. Open Questions & Discussion Points

-   Provide a section for any questions that have arisen or discussion points that need input from the stakeholders.

### Frequency of Updates

-   During critical phases (active containment, major impact): Daily or even multiple times a day.
-   During recovery/post-incident: Less frequent (e.g., every 2-3 days, weekly).
-   Establish a clear schedule at the beginning of the incident and stick to it.

### Tone and Level of Detail

-   **Tone:** Professional, factual, calm, and confident. Avoid speculation.
-   **Detail:** Balance between providing enough information for informed decisions and avoiding overwhelming technical jargon. Translate technical actions into business impact.

### Example Template Structure (Markdown)

```markdown
# INCIDENT STATUS UPDATE: Ransomware Attack - Production Database

**Update Number:** #3
**Date/Time of Update:** 2026-01-29 20:00 UTC
**Reporting Period:** 2026-01-29 16:00 UTC - 2026-01-29 20:00 UTC
**Prepared By:** Incident Response Team

---

## 1. Incident Summary Refresh

As previously communicated, on [Date], our systems were impacted by a ransomware attack that encrypted our primary production database cluster and several web servers.

## 2. Current Status & Key Developments

**Overall Status:** Incident is contained, and recovery from backups is progressing.

**Key Developments:**
-   **Positive:** Restoration of the core production database is now 60% complete, up from 40% in the last update.
-   **Positive:** Initial scans of unaffected systems confirm no lateral movement of the ransomware.
-   **Challenge:** Identified a minor compatibility issue with a legacy application post-restore, which is being addressed by the applications team. This may add a slight delay to full operational readiness.

## 3. Impact Assessment

**Current Business Impact:**
-   Customer-facing application remains offline. Users are seeing a maintenance page.
-   Internal reporting for sales and finance remains impacted.
-   **Revised Recovery Target:** Full operational recovery now estimated by 2026-01-30 12:00 UTC (previously 08:00 UTC) due to the identified compatibility issue.
**Financial Implications:** Current cost estimates for recovery (third-party experts, cloud resources) are holding at $60,000. Revenue loss estimates are being re-evaluated based on the revised recovery timeline.
**Reputational Risks:** PR team is on standby. Customer communication drafted for release upon service restoration.

## 4. Actions Taken & Mitigation Efforts

**Actions during Reporting Period:**
-   Continued database restoration activities.
-   Assisted applications team in troubleshooting legacy application compatibility.
-   Expanded threat hunting across additional internal network segments (no further compromise detected).
-   Implemented a temporary read-only reporting solution for critical business units.

**Mitigation Effectiveness:** Containment measures are holding. Ransomware has been eradicated from affected systems prior to restoration.

## 5. Next Steps & Forward Plan

**Immediate Priorities:**
-   Complete primary database restoration and full application functionality testing.
-   Resolve legacy application compatibility issue.
-   Validate integrity of restored data.
-   Prepare for controlled re-enablement of customer-facing services.

**Expected Outcomes by Next Update (2026-01-30 08:00 UTC):**
-   Database restoration 90%+ complete.
-   Compatibility issue resolution in progress or complete.
-   Preliminary plan for service re-enablement finalized.

## 6. Open Questions & Discussion Points

-   Request for C-level confirmation on the revised service restoration timeline.
-   Discussion with legal regarding potential vendor notification given the identified root cause.

---
**Point of Contact:** [Name], Head of Security Operations
```
