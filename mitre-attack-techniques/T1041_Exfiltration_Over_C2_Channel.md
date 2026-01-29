# T1041: Exfiltration Over C2 Channel

## Summary

Exfiltration Over C2 Channel involves adversaries stealing data by sending it out of the compromised network over an existing command and control (C2) channel. This technique uses the established C2 communication path, often disguised as normal traffic, to transfer sensitive information without being detected.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1041.001 | Data Transfer | Exfiltrating data directly over C2 |
| T1041.002 | Encrypted Data | Encrypting data before exfiltration |
| T1041.003 | Compressed Data | Compressing data before exfiltration |

## How It's Used by Adversaries

- **Data Theft**: Stealing sensitive information from the organization.
- **Maintaining Cover**: Blending exfiltration with legitimate C2 traffic.
- **Evading Detection**: Using encrypted or compressed formats.

## What to Look For (Detection)

- Unusual outbound network connections to known malicious IPs or domains.
- High volume of data transferred over C2 channels, especially encrypted/compressed data.
- Anomalous traffic patterns (e.g., data leaving during off-hours).
- Unexpected processes initiating outbound connections.

## Detection Rules

### Splunk Query - High Volume Outbound from Internal Host
```
index=firewall action=allowed direction=outbound
| stats sum(bytes_out) as total_bytes_out by dest_ip, src_ip, user
| where total_bytes_out > 100000000
| `security_content_annotations`
| fillnull value="-" total_bytes_out
```

### Sigma Rule - Suspicious C2 Exfiltration
```yaml
title: Suspicious Outbound Connection to Rare Destination
description: Detects outbound connections to rarely seen external IPs, indicative of C2 or exfiltration.
logsource:
  category: network_connection
  product: windows
  service: sysmon
detection:
  selection:
    Initiated: 'true'
    DestinationIp|!startswith:
      - '10.'
      - '172.16.'
      - '172.17.'
      - '172.18.'
      - '172.19.'
      - '172.20.'
      - '172.21.'
      - '172.22.'
      - '172.23.'
      - '172.24.'
      - '172.25.'
      - '172.26.'
      - '172.27.'
      - '172.28.'
      - '172.29.'
      - '172.30.'
      - '172.31.'
      - '192.168.'
      - '127.0.0.1'
    DestinationPort:
      - 80
      - 443
  timeframe: 1h
  condition: selection | group by Image, DestinationIp count() > 5
  falsepositives:
    - Cloud services
    - Legitimate external communication
level: high
```

## Response Actions

1.  **Block C2**: Immediately block communication to the identified C2 infrastructure.
2.  **Isolate Hosts**: Isolate systems confirmed to be exfiltrating data.
3.  **Analyze Data**: Determine what data was exfiltrated and its sensitivity.
4.  **Remediate**: Remove C2 malware, patch vulnerabilities, and strengthen data loss prevention.

## MITRE ATT&CK Mapping

| Technique | Tactics | Detection |
|-----------|---------|-----------|
| Exfiltration Over C2 Channel | Exfiltration | High volume outbound traffic, unusual C2 communications |

## References

- [MITRE ATT&CK T1041](https://attack.mitre.org/techniques/T1041/)
- [Detecting Data Exfiltration](https://example.com/detecting-data-exfiltration)

---
*MITRE ATT&CK: T1041 - Exfiltration Over C2 Channel*
*Last Updated: 2026-01-29*
