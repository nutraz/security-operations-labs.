# IOC Enrichment Workflow

---

### 1. Introduction & Importance of IOC Enrichment

An Indicator of Compromise (IOC) is a piece of forensic data found in a system or network that indicates, with high confidence, a computer intrusion. While initial detection of an IOC (e.g., a suspicious IP address, a malicious file hash) is crucial, its full value is unlocked through **enrichment**.

IOC enrichment is the process of gathering additional context and intelligence about a detected IOC from various sources. This process transforms raw data points into actionable intelligence, enabling faster and more informed incident response decisions.

**Importance:**
-   **Contextualization:** Understand the nature, origin, and potential impact of a threat.
-   **Prioritization:** Help security teams prioritize which alerts and threats to investigate first.
-   **Threat Hunting:** Identify related IOCs and pivot to find other compromised systems or attacker infrastructure.
-   **Improved Detections:** Use enriched data to create more robust and precise detection rules.
-   **Incident Response Acceleration:** Provide critical information for blocking, containment, and eradication efforts.

### 2. Common IOC Types

-   **IP Addresses:** Malicious C2 servers, phishing sites, botnet members.
-   **Domain Names:** Malicious domains, typosquatted domains, phishing domains.
-   **File Hashes (MD5, SHA1, SHA256):** Malicious executables, documents, scripts.
-   **URLs:** Phishing links, malware download links, exploit kit landing pages.
-   **Email Addresses/Subjects:** Phishing senders, malicious email subjects.
-   **Registry Keys/Values:** Persistence mechanisms, malware configurations.
-   **File Paths:** Locations where malware is dropped or executes from.

### 3. Data Sources for Enrichment

Various external and internal sources can provide valuable context for IOCs.

-   **Reputation Services/Threat Intelligence Platforms (TIPs):**
    -   **VirusTotal:** Aggregates antivirus scan results, file behaviors, and community comments.
    -   **AlienVault OTX (Open Threat Exchange):** Community-driven threat intelligence sharing platform.
    -   **Pulsedive, ANY.RUN, Joe Sandbox:** Malware analysis sandboxes that provide dynamic analysis reports.
-   **Passive DNS (pDNS):** Historical DNS records showing what IP addresses a domain resolved to over time, and what domains resolved to a given IP.
-   **WHOIS Records:** Provides registration details for domain names and IP addresses, including registrant contact information, creation dates, and nameservers.
-   **URL Scanners:** Services that analyze URLs for malicious content without actually visiting them (e.g., Google Safe Browsing, URLscan.io).
-   **Internal Logs:**
    -   **SIEM/Log Management:** Search for other occurrences of the IOC within your environment.
    -   **Proxy Logs/Firewall Logs:** Check if other internal hosts have communicated with the IOC.
    -   **EDR Data:** Query endpoint detection and response systems for process activity, network connections, or file modifications related to the IOC.

### 4. Workflow Steps

An effective IOC enrichment workflow often involves both automated and manual steps.

1.  **Initial Detection & Alert:** An alert is triggered by a SIEM, EDR, or other security control, identifying a potential IOC (e.g., a network connection to `45.67.89.123` as seen in our `memory-analysis-walkthrough.md`).
2.  **Automated Enrichment (API-driven):**
    -   Automatically query reputable threat intelligence sources (e.g., VirusTotal API for file hashes, AbuseIPDB for IP reputation).
    -   Integrate with internal systems (e.g., query SIEM for historical occurrences).
    -   Augment the initial alert with the enriched data.
3.  **Contextual Analysis:**
    -   **Reputation Scores:** Is the IOC known malicious? What is its threat score?
    -   **Associated Malware:** What malware families are linked to this IOC?
    -   **Campaign Attribution:** Is it part of a known threat actor group or campaign?
    -   **Geographic Location:** Where is the IP/server located?
    -   **Timestamps:** When was the domain registered? When did the IP last resolve to a malicious domain?
    -   **Related IOCs:** What other IOCs are associated with this one? (e.g., other IPs/domains from the same threat actor).
4.  **Manual Deep Dive (Analyst Review):**
    -   If automated enrichment is insufficient or yields ambiguous results, a human analyst performs deeper investigation.
    -   This may involve manual lookups in specialized databases, sandboxing suspicious files, or analyzing network traffic.
    -   For a suspicious domain, check WHOIS records for recent changes or privacy services.
5.  **Correlation & Pivoting:**
    -   Use the enriched data to pivot and search for related activities or IOCs within the environment.
    -   For example, if an IP is linked to a specific malware family, search for other indicators of that malware.
6.  **Action & Remediation:**
    -   Based on the enriched intelligence, take appropriate actions: block the IOC at the firewall/proxy, hunt for further compromise, update detection rules, initiate incident response.
7.  **Documentation:** Document all findings and actions taken for future reference and knowledge sharing.

### 5. Tools & Platforms

-   **Threat Intelligence Platforms (TIPs):** Centralize IOC management, enrichment, and sharing (e.g., MISP, Anomali ThreatStream).
-   **SOAR Platforms:** Orchestrate automated enrichment workflows and integrate with various TI sources.
-   **Custom Python Scripts:** For interacting with various TI APIs.
-   **Browser Extensions:** For quick lookups of IPs/domains (e.g., from network captures).

### 6. Integration with SIEM/EDR

-   **SIEM:** Ingest enriched IOC data directly into your SIEM to enhance correlation rules and provide context for alerts. Use enriched data for threat hunting queries.
-   **EDR:** Push validated malicious IOCs to your EDR to improve endpoint detection and response capabilities (e.g., blocking processes, isolating hosts).

---
*Effective IOC enrichment is an iterative process that continually refines threat understanding and enhances defensive capabilities.*
