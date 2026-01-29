# T1566: Phishing

## Summary

Phishing is a social engineering technique where adversaries attempt to trick users into revealing sensitive information or performing actions that compromise security. It is the most common initial access vector for threat actors.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1566.001 | Spearphishing Attachment | Malicious attachments in targeted emails |
| T1566.002 | Spearphishing Link | Malicious links in targeted emails |
| T1566.003 | Spearphishing by Service | Phishing via third-party services (social media, messaging) |

## How It's Used by Adversaries

- **Mass Campaign Phishing**: Wide-spread emails targeting organization employees
- **Spearphishing**: Highly targeted attacks against specific individuals
- **Business Email Compromise (BEC)**: Impersonating executives to trick employees
- **Credential Harvesting**: Fake login pages to capture credentials
- **Malware Delivery**: Attachments containing malicious payloads

## What to Look For (Detection)

### Email Header Analysis
```
- Check SPF/DKIM/DMARC results
- Analyze Return-Path vs From address
- Examine Received headers for path tracing
- Look for mismatched domains
```

### Content Indicators
```
- Urgency and pressure tactics
- Generic greetings
- Spelling/grammar errors
- Suspicious attachments (.zip, .exe, .js, .vbs)
- Malicious URLs (typosquatting, shorteners)
```

### Sandbox Analysis
```
- Detonate attachments in isolated environment
- Monitor for process execution
- Check network connections to C2
- Analyze dropped files
```

## Example Attack Chain

```
1. Email received with invoice.zip
   - From: accounts@compnay.com (typosquatting)
   - Subject: URGENT: Unpaid Invoice
   
2. User opens ZIP file
   - Contains invoice_details.js
   
3. JavaScript executes PowerShell
   - Downloads payload.exe from C2
   
4. Payload establishes persistence
   - Creates scheduled task
   - Downloads additional modules
   
5. Data exfiltration begins
   - Collects credentials
   - Exfiltrates to external server
```

## Detection Rules

### Splunk Query
```
index=email 
| search "invoice" OR "payment" OR "urgent"
| where sender != "*@legitimate-domain.com"
| stats count by sender, subject, recipient
```

### Sigma Rule
```yaml
title: Suspicious Email Attachment
description: Detects emails with suspicious attachment types
detection:
  selection:
    AttachmentExtension:
      - '.zip'
      - '.exe'
      - '.js'
      - '.vbs'
      - '.scr'
  condition: selection
level: medium
```

## Response Actions

1. **Immediate Containment**
   - Block sender domain/IP
   - Delete emails from mail server
   - Isolate affected endpoints

2. **Investigation**
   - Analyze email headers
   - Sandbox attachment
   - Extract IOCs

3. **Remediation**
   - Reset compromised credentials
   - Rebuild affected systems
   - Update email filters

4. **Prevention**
   - User awareness training
   - Email gateway rules
   - DMARC/SPF/DKIM enforcement

## References

- [MITRE ATT&CK T1566](https://attack.mitre.org/techniques/T1566/)
- [Phishing Prevention Guide](https://www.cisa.gov/phishing)

---
*MITRE ATT&CK: T1566 - Phishing*
*Last Updated: 2026-01-28*

