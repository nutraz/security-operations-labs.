# Sample Risk Register (Aligned to ISO 27001)

This document provides a simplified example of a risk register for tracking information security risks.

---

| ID  | Date Identified | Risk Title              | Risk Description                                                                                             | Annex A Control | Initial Likelihood (1-5) | Initial Impact (1-5) | Initial Risk Score (L*I) | Control Effectiveness (1-5) | Residual Likelihood (1-5) | Residual Impact (1-5) | Residual Risk Score (L*I) | Risk Owner  | Treatment Plan                                  | Status      |
|-----|-----------------|-------------------------|--------------------------------------------------------------------------------------------------------------|-----------------|--------------------------|----------------------|--------------------------|-----------------------------|---------------------------|-----------------------|---------------------------|-------------|-------------------------------------------------|-------------|
| R001| 2026-01-15      | Ransomware Infection    | Critical servers could be encrypted by a ransomware attack, leading to significant business disruption and data loss. | A.12.2.1        | 3 (Possible)             | 5 (Critical)         | 15                       | 4 (High)                    | 1 (Rare)                  | 2 (Minor)             | 2                         | Head of IT  | **Mitigate:** EDR, Backups, IR Playbook, Awareness      | In Progress |
| R002| 2026-01-18      | Phishing Attack         | Employees may be tricked by phishing emails, leading to credential theft and unauthorized access to systems.      | A.7.2.2         | 4 (Likely)               | 4 (Major)            | 16                       | 3 (Medium)                  | 2 (Unlikely)              | 3 (Moderate)          | 6                         | SOC Manager | **Mitigate:** Awareness Training, Email Filtering, MFA | In Progress |
| R003| 2026-01-20      | Unpatched Web Server    | A public-facing web server has a critical vulnerability (e.g., Log4j) that could be exploited by attackers.    | A.12.6.1        | 5 (Almost Certain)       | 5 (Critical)         | 25                       | 5 (Very High)               | 1 (Rare)                  | 1 (Negligible)        | 1                         | Web Admin   | **Mitigate:** Patch Mgmt, WAF, Vulnerability Scanning | Completed   |
| R004| 2026-01-22      | Data Exfiltration       | A privileged insider or an external attacker could steal sensitive customer data from the main database.         | A.8.2.3, A.9.4.4 | 2 (Unlikely)             | 5 (Critical)         | 10                       | 2 (Low)                     | 2 (Unlikely)              | 4 (Major)             | 8                         | CISO        | **Mitigate:** DLP, DAM, Access Reviews, Auditing | Planned     |
| R005| 2026-01-25      | Third-Party API Failure | A critical business process relies on a third-party API that could become unavailable, halting operations.     | A.15.1.2        | 3 (Possible)             | 3 (Moderate)         | 9                        | 1 (Very Low)                | 3 (Possible)              | 3 (Moderate)          | 9                         | App Owner   | **Accept:** Current SLA, Contingency Plan, Monitoring | Accepted    |

---

### Legend

**Likelihood Scale (Initial & Residual):**
- **1:** Rare (Unlikely to occur)
- **2:** Unlikely (May occur once every 5+ years)
- **3:** Possible (May occur once every 1-5 years)
- **4:** Likely (May occur once a year)
- **5:** Almost Certain (Will occur multiple times a year)

**Impact Scale (Initial & Residual):**
- **1:** Negligible (Minor inconvenience, no business impact)
- **2:** Minor (Small disruption, minimal financial loss)
- **3:** Moderate (Significant disruption, moderate financial loss, reputational damage)
- **4:** Major (Severe disruption, major financial loss, significant reputational damage, legal/regulatory impact)
- **5:** Critical (Catastrophic disruption, existential threat to business)

**Control Effectiveness Scale:**
- **1:** Very Low (Controls are largely ineffective or absent)
- **2:** Low (Controls exist but have significant weaknesses)
- **3:** Medium (Controls are moderately effective, some weaknesses remain)
- **4:** High (Controls are very effective, minor weaknesses)
- **5:** Very High (Controls are highly effective, no known weaknesses)

**Risk Treatment Options:**
- **Mitigate:** Apply security controls to reduce likelihood or impact.
- **Accept:** Formally acknowledge the risk and accept it without further action.
- **Transfer:** Transfer the risk to a third party (e.g., via cyber insurance).
- **Avoid:** Change business processes to avoid the risk entirely.

---

### Residual Risk Calculation Methodology

Residual risk is the risk that remains after security controls have been implemented and are operating effectively. It is calculated by assessing the Likelihood and Impact of a risk *after* controls have been applied, rather than before.

#### Steps:

1.  **Determine Initial Risk:** Assess the Likelihood and Impact of the risk *before* any controls are applied (or if controls were completely absent).
    -   `Initial Risk Score = Initial Likelihood × Initial Impact`
2.  **Evaluate Control Effectiveness:** Assess how effective the implemented (or planned) controls are at reducing the likelihood or impact of the risk. Use the "Control Effectiveness Scale."
3.  **Calculate Residual Likelihood:** Adjust the Initial Likelihood based on the Control Effectiveness. For example:
    -   If a control is Very High (5), it might reduce a Likelihood of 5 to 1.
    -   If a control is Medium (3), it might reduce a Likelihood of 5 to 3.
    *This is often a subjective adjustment based on the control's ability to prevent the risk event.*
4.  **Calculate Residual Impact:** Adjust the Initial Impact based on the Control Effectiveness, particularly for controls designed to minimize damage if an event occurs (e.g., backups reducing impact of ransomware).
    *This is often a subjective adjustment based on the control's ability to reduce the consequences.*
5.  **Determine Residual Risk Score:**
    -   `Residual Risk Score = Residual Likelihood × Residual Impact`

#### Example (R001 - Ransomware Infection):

-   **Initial Likelihood:** 3 (Possible)
-   **Initial Impact:** 5 (Critical)
-   **Initial Risk Score:** 15
-   **Control Effectiveness:** 4 (High - due to EDR, tested backups, and IR playbook)
-   **Residual Likelihood:** Controls like EDR and user awareness significantly reduce the chance of initial infection. Reduced from 3 to 1.
-   **Residual Impact:** Tested backups and an IR playbook significantly reduce the impact if an infection occurs. Reduced from 5 to 2.
-   **Residual Risk Score:** `1 (Residual Likelihood) × 2 (Residual Impact) = 2`

This shows that while the initial risk was high, the implemented controls (EDR, Backups, IR Playbook, Awareness) effectively reduced the residual risk to a much lower level.
