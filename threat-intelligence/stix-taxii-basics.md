# STIX/TAXII Basics Document

---

### 1. Introduction to STIX and TAXII

In the realm of cyber threat intelligence (CTI), effective sharing and consumption of information are paramount. The Structured Threat Information eXpression (STIX) and Trusted Automated eXchange of Indicator Information (TAXII) are two related standards designed to facilitate this. Together, they provide a structured language for describing CTI and a secure mechanism for exchanging it.

-   **STIX:** Defines *what* threat intelligence looks like. It's a standardized language and serialization format for describing CTI in a structured, machine-readable way.
-   **TAXII:** Defines *how* threat intelligence is shared. It's a set of services and message exchanges that enable automated sharing of CTI.

### 2. What is STIX?

STIX is designed to convey the full range of CTI, including indicators, attack patterns, malware, threat actors, campaigns, incidents, and more. It aims to be expressive, flexible, and extensible.

**Key Concepts in STIX (version 2.x):**

-   **STIX Domain Objects (SDOs):** Represent the fundamental concepts of CTI. Examples include:
    -   `Attack-Pattern`: A type of tactic, technique, or procedure (TTP) used by adversaries.
    -   `Campaign`: A set of malicious behaviors and/or attacks that are attributed to a common adversary and a common intent.
    -   `Indicator`: A pattern of suspicious or malicious cyber activity that can be used to detect potential adversaries. (e.g., a malicious IP address, a file hash).
    -   `Malware`: A type of malicious software.
    -   `Threat-Actor`: An individual or group responsible for malicious cyber activity.
    -   `Intrusion-Set`: A grouped set of adversarial behaviors and resources with a known objective.
    -   `Report`: A collection of threat intelligence that conveys a common understanding.
-   **STIX Cyber Observables (SCOs):** Describe concrete pieces of information observed on systems or networks. Examples include:
    -   `File`: A file observable.
    -   `IPv4-Address`: An IPv4 address observable.
    -   `Domain-Name`: A domain name observable.
    -   `URL`: A URL observable.
-   **Relationships:** STIX objects are interconnected through relationships, forming a graph of threat intelligence. For example:
    -   An `Indicator` *detects* a `Malware`.
    -   A `Threat-Actor` *uses* an `Attack-Pattern`.
    -   A `Campaign` *targets* a `Vulnerability`.

**Example STIX Indicator (simplified JSON):**
```json
{
  "type": "indicator",
  "id": "indicator--a7bb3b00-a61f-4f24-9b3b-8f78d9b1a0e1",
  "spec_version": "2.1",
  "created": "2026-01-29T10:00:00.000Z",
  "modified": "2026-01-29T10:00:00.000Z",
  "pattern": "[ipv4-addr:value = '45.67.89.123']",
  "pattern_type": "stix",
  "valid_from": "2026-01-29T10:00:00.000Z",
  "description": "Indicates network connection to known C2 IP from phishing campaign."
}
```

### 3. What is TAXII?

TAXII defines a set of API-enabled services that implement various sharing models for CTI. It provides a client-server architecture for producers and consumers of threat intelligence to exchange STIX-formatted data.

**Key Concepts in TAXII (version 2.x):**

-   **Collections:** Resources that hold STIX objects. A producer can make multiple collections available, each containing different types or sources of CTI. Consumers can subscribe to specific collections.
-   **API Root:** The entry point for interacting with a TAXII server, hosting one or more collections.
-   **Discovery Service:** Allows clients to discover the available API Roots on a TAXII server.
-   **Collections Service:** Lists the collections available at an API Root.
-   **Objects Service:** Allows clients to retrieve STIX objects from a collection.
-   **Channels (deprecated in TAXII 2.x, but concept exists in earlier versions):** A mechanism for pushing or polling CTI. TAXII 2.x primarily uses polling.
-   **Authentication:** TAXII servers typically require authentication (e.g., API keys, client certificates) to access collections.

**TAXII Workflow (Simplified Polling):**
1.  **Discovery:** CTI Consumer finds the API Root(s) of a CTI Producer.
2.  **Collection Discovery:** CTI Consumer queries an API Root to see available Collections.
3.  **Polling:** CTI Consumer sends a request to a specific Collection to retrieve new STIX objects since its last poll.

### 4. Benefits of using STIX/TAXII for CTI Sharing

-   **Standardization:** Provides a common language and format, reducing ambiguity and improving interoperability between different security tools and organizations.
-   **Automation:** Enables automated ingestion and processing of threat intelligence, accelerating defense.
-   **Context Richness:** STIX's structured nature allows for detailed and contextualized threat information, not just raw indicators.
-   **Relationship Mapping:** Facilitates understanding the connections between different threat components (e.g., which malware uses which attack pattern, which threat actor is behind a campaign).
-   **Improved Collaboration:** Promotes sharing and collaboration among security communities and within an organization.

### 5. How STIX/TAXII Fits into a Threat Intelligence Program

-   **Consumption:** Security tools (SIEMs, EDRs, TIPs) can act as TAXII clients to automatically ingest STIX-formatted threat intelligence from subscribed feeds.
-   **Production:** Organizations can produce their own STIX content (e.g., based on internal incident analysis) and share it via a TAXII server with trusted partners or internal systems.
-   **Analysis & Enrichment:** STIX objects can be processed by threat intelligence platforms for further analysis, enrichment, and correlation with internal data.

---
*Understanding STIX and TAXII is fundamental for building mature, automated, and collaborative threat intelligence capabilities.*
