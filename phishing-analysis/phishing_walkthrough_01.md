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

-   **Display Name vs. Actual Address:** The "From" address `accounts.payable@compnay.com` is a **spoof**. The `a` is missing from "company".
-   **Analyze Email Headers:**
    -   Look at the `Received` headers to trace the email's path. Does it originate from a trusted mail server associated with the legitimate company?
    -   Check the `Return-Path` or `Reply-To` header. Often, this will be the attacker's actual email address.
    -   Check SPF/DKIM/DMARC results in the headers. A legitimate email should pass these checks. This one will likely show `SPF: SoftFail` or `Fail`.
-   **Conclusion:** The sender is impersonated. This is a classic phishing indicator.

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
    -   The PowerShell command downloads another file (`payload.exe`) from a suspicious URL (`http://totally-legit-files.xyz/payload.exe`).
    -   `payload.exe` is identified by multiple AV engines as a known trojan (e.g., Agent Tesla, Qakbot).
    -   The payload attempts to make network connections to a C2 server at `45.67.89.123`.

#### Step 4: Indicator of Compromise (IOC) Extraction

From the analysis, we have the following IOCs:

-   **Email Sender:** `accounts.payable@compnay.com`
-   **Email Subject:** "Urgent: Unpaid Invoice INV-08736"
-   **Attachment Hash (SHA256):** `[hash_of_Invoice_INV-08736.zip]`
-   **File Hash (SHA256):** `[hash_of_invoice_details.js]`
-   **File Hash (SHA256):** `[hash_of_payload.exe]`
-   **Malicious URL:** `http://totally-legit-files.xyz/payload.exe`
-   **C2 IP Address:** `45.67.89.123`

### 3. Response & Remediation

1.  **Block IOCs:** Add the file hashes, URL, and IP address to the appropriate security tools (EDR, proxy, firewall).
2.  **Email Search & Purge:** Search the mail server for all emails matching the sender/subject and delete them from user inboxes.
3.  **User Communication:** Alert users who may have received the email and instruct them not to open it. If anyone reports opening the attachment, trigger an incident response for their workstation.
4.  **Report:** Document the findings and actions taken.
