# Detection Rules Overview

This directory contains detection rules and queries for various security tools including Splunk, Sigma, and KQL (Microsoft Sentinel).

## Directory Structure

```
detection-rules/
├── splunk-queries/
│   ├── brute-force-detection.spl
│   ├── powershell-execution.spl
│   └── phishing-indicators.spl
├── sigma-rules/
│   ├── brute-force-detection.yml
│   ├── powershell-encoded-command.yml
│   └── suspicious-network-connections.yml
├── kql-queries/
│   ├── brute-force-detection.kql
│   ├── powershell-execution.kql
│   └── phishing-analysis.kql
└── README.md
```

## Usage Guidelines

### Splunk Queries
1. Copy the `.spl` file content
2. Paste into Splunk Search app
3. Adjust time range as needed
4. Customize field extractions if required

### Sigma Rules
1. Use Sigma CLI converter: `sigmatools convert -r sigma-rules/ -t splunk -o splunk-queries/`
2. Or use online converter at https://uncoder.io/
3. Test in your SIEM environment

### KQL Queries
1. Copy to Microsoft Sentinel/Log Analytics
2. Adjust table names if using custom logs
3. Create detection alerts based on results

## MITRE ATT&CK Mapping

| Technique | Sigma Rule | Splunk Query | KQL Query |
|-----------|-----------|--------------|-----------|
| T1110 Brute Force | brute-force-detection.yml | brute-force-detection.spl | brute-force-detection.kql |
| T1059.001 PowerShell | powershell-encoded-command.yml | powershell-execution.spl | powershell-execution.kql |
| T1566 Phishing | (in development) | phishing-indicators.spl | phishing-analysis.kql |

## Tuning Recommendations

1. **False Positive Reduction**
   - Add user agent filtering
   - Implement time-based throttling
   - Exclude known good processes

2. **Alert Prioritization**
   - Map to asset criticality
   - Factor in user privilege level
   - Consider attack stage (ATT&CK)

3. **Response Actions**
   - Auto-block IPs after N failures
   - Generate tickets automatically
   - Alert on-call personnel for high-severity

---
*Last Updated: 2026-01-28*

