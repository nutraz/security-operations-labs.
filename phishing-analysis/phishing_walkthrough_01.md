# Phishing Analysis Walkthrough: "Invoice INV-08736"

This document walks through the analysis of a suspicious email.

---

### 1. Initial Email Details

-   **From:** `accounts.payable@compnay.com` (note the spelling "compnay")
-   **Subject:** Urgent: Unpaid Invoice INV-08736
-   **Attachment:** `Invoice_INV-08736.zip`
-   **Body:** "Dear team, please see attached invoice for immediate payment. Failure to pay will result in service suspension. Regards, John"

### 2. Analysis Steps

#### Step 1: Sender & Header Analysis

Email headers contain a wealth of information crucial for determining an email's legitimacy and origin.

-   **Display Name vs. Actual Address:** The "From" address `accounts.payable@compnay.com` is a **spoof**. The `a` is missing from "company". Always verify the actual email address, not just the display name.
-   **Analyze Email Headers (Key Headers to Examine):**
    -   `Received`: These headers show the path an email took from sender to recipient. Read them bottom-up. Look for:
        -   Unusual sending IP addresses or domains.
        -   Discrepancies between the sending server and the purported sender's domain.
        -   Too many hops, or unexpected internal servers.
    -   `Return-Path` or `Reply-To`: Often reveals the true sender's email address, which might differ from the "From" address, indicating spoofing.
    -   `Authentication-Results`: This header provides the results of SPF, DKIM, and DMARC checks.
        -   `SPF (Sender Policy Framework)`: Checks if the sending IP is authorized by the sender's domain. `SoftFail`, `Fail`, or `Neutral` are red flags.
        -   `DKIM (DomainKeys Identified Mail)`: Verifies the sender's identity and ensures the email hasn't been tampered with. `Fail` indicates a problem.
        -   `DMARC (Domain-based Message Authentication Authentication, Reporting & Conformance)`: Combines SPF and DKIM. A `Fail` indicates a likely spoofed email.
    -   `X-Originating-IP` or `X-Mailer`: Can sometimes reveal the actual IP address of the sender's client.
    -   `Message-ID`: A unique identifier for the email; unusual formats can sometimes be a red flag.
-   **Conclusion:** The sender is impersonated, and header analysis (especially SPF/DKIM/DMARC failures) confirms this. This is a classic phishing indicator.

#### Step 2: Content & Tone Analysis

-   **Urgency:** The email uses words like "Urgent" and "immediate payment" to create a sense of pressure, hoping the recipient will act without thinking.
-   **Generic Salutation:** "Dear team" is impersonal. A real accounts payable email would likely address a specific person or department.
-   **Grammar/Spelling:** "compnay.com" is a clear typo. Attackers often make these mistakes.

#### Step 3: Attachment Analysis (Sandboxing)

-   **File Type:** The attachment is a `.zip` file. This is suspicious, as invoices are typically sent as PDFs. ZIP files are often used to hide malicious executables.
-   **Action: Detonate in a Sandbox.**
    -   Upload the `.zip` file to a sandbox environment (e.g., a local VM, or an online service like VirusTotal, Any.Run, or Joe Sandbox). **DO NOT OPEN IT ON YOUR WORKSTATION.**
-   **Sandbox Results:**
    -   The `.zip` file contains a file named `invoice_details.js` (a JavaScript file).
    -   When executed, `invoice_details.js` runs a PowerShell command.
    -   The PowerShell command downloads another file (`payload.exe`) from a suspicious URL (`https://totally-legit-files.xyz/payload.exe`).
    -   `payload.exe` is identified by multiple AV engines as a known trojan (e.g., Agent Tesla, Qakbot).
    -   The payload attempts to make network connections to a C2 server at `45.67.89.123`.

#### Step 4: Indicator of Compromise (IOC) Extraction

From the analysis, we have the following IOCs:

-   **Email Sender:** `accounts.payable@compnay.com`
-   **Email Subject:** "Urgent: Unpaid Invoice INV-08736"
-   **Attachment Hash (SHA256):** `[hash_of_Invoice_INV-08736.zip]`
-   **File Hash (SHA256):** `[hash_of_invoice_details.js]`
-   **File Hash (SHA256):** `[hash_of_payload.exe]`
-   **Malicious URL:** `https://totally-legit-files.xyz/payload.exe`
-   **C2 IP Address:** `45.67.89.123`

### 3. Response & Remediation

1.  **Block IOCs:** Add the file hashes, URL, and IP address to the appropriate security tools (EDR, proxy, firewall).
2.  **Email Search & Purge:** Search the mail server for all emails matching the sender/subject and delete them from user inboxes.
3.  **User Communication:** Alert users who may have received the email and instruct them not to open it. If anyone reports opening the attachment, trigger an incident response for their workstation.
4.  **Report:** Document the findings and actions taken.
