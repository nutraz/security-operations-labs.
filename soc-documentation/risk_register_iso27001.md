# Sample Risk Register (Aligned to ISO 27001)

This document provides a simplified example of a risk register for tracking information security risks.

---

| ID  | Date Identified | Risk Title              | Risk Description                                                                                             | Annex A Control | Likelihood (1-5) | Impact (1-5) | Risk Score (L*I) | Risk Owner  | Treatment Plan                                  | Status      |
|-----|-----------------|-------------------------|--------------------------------------------------------------------------------------------------------------|-----------------|------------------|--------------|--------------------|-------------|-------------------------------------------------|-------------|
| R001| 2026-01-15      | Ransomware Infection    | Critical servers could be encrypted by a ransomware attack, leading to significant business disruption and data loss. | A.12.2.1        | 3 (Possible)     | 5 (Critical) | 15                 | Head of IT  | **Accept:** Mitigate via EDR, Backups, & IR Playbook | In Progress |
| R002| 2026-01-18      | Phishing Attack         | Employees may be tricked by phishing emails, leading to credential theft and unauthorized access to systems.      | A.7.2.2         | 4 (Likely)       | 4 (Major)    | 16                 | SOC Manager | **Mitigate:** Security Awareness Training & Email Filtering | In Progress |
| R003| 2026-01-20      | Unpatched Web Server    | A public-facing web server has a critical vulnerability (e.g., Log4j) that could be exploited by attackers.    | A.12.6.1        | 5 (Almost Certain) | 5 (Critical) | 25                 | Web Admin   | **Mitigate:** Apply security patches immediately.     | Completed   |
| R004| 2026-01-22      | Data Exfiltration       | A privileged insider or an external attacker could steal sensitive customer data from the main database.         | A.8.2.3, A.9.4.4 | 2 (Unlikely)     | 5 (Critical) | 10                 | CISO        | **Mitigate:** Implement DLP and database activity monitoring. | Planned     |
| R005| 2026-01-25      | Third-Party API Failure | A critical business process relies on a third-party API that could become unavailable, halting operations.     | A.15.1.2        | 3 (Possible)     | 3 (Moderate) | 9                  | App Owner   | **Accept:** Current SLA is sufficient. Develop contingency plan. | Accepted    |

---

### Legend

**Likelihood Scale:**
- **1:** Rare
- **2:** Unlikely
- **3:** Possible
- **4:** Likely
- **5:** Almost Certain

**Impact Scale:**
- **1:** Negligible
- **2:** Minor
- **3:** Moderate
- **4:** Major
- **5:** Critical

**Risk Treatment Options:**
- **Mitigate:** Apply security controls to reduce likelihood or impact.
- **Accept:** Formally acknowledge the risk and accept it without further action.
- **Transfer:** Transfer the risk to a third party (e.g., via cyber insurance).
- **Avoid:** Change business processes to avoid the risk entirely.
