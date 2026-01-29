# T1003: Credential Dumping

## Summary

Credential Dumping is the act of stealing account access information, such as usernames and passwords, from a computer's memory or local storage. Adversaries use this information to gain access to additional systems and resources within a network.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1003.001 | LSASS Memory | Attacking LSASS to dump credentials |
| T1003.002 | Security Account Manager (SAM) | Extracting credentials from SAM database |
| T1003.003 | NTDS.dit | Dumping credentials from Active Directory |
| T1003.004 | LSA Secrets | Accessing LSA Secrets for stored credentials |
| T1003.005 | Cached Domain Credentials | Extracting cached domain credentials |
| T1003.006 | Group Policy Preferences | Retrieving credentials from GPP files |

## How It's Used by Adversaries

- **Pass-the-Hash/Ticket**: Using dumped credentials for lateral movement.
- **Persistent Access**: Maintaining access to compromised systems.
- **Privilege Escalation**: Gaining higher-level access.

## What to Look For (Detection)

- Access to LSASS process memory.
- Reads from SAM registry hive.
- Execution of credential dumping tools (e.g., Mimikatz, procdump).
- Unusual access to `NTDS.dit` file.

## Detection Rules

### Splunk Query - LSASS Access
```
index=windows EventCode=4656 ObjectType="LSASS" AccessMask="0x1000"
| stats count by _time, host, EventCode, ObjectName, AccessMask
| `security_content_annotations`
| fillnull value="-" _time
```

### Sigma Rule - Mimikatz Usage
```yaml
title: Mimikatz Process Creation
description: Detects the creation of processes commonly associated with Mimikatz.
logsource:
  product: windows
  service: security
detection:
  selection:
    ParentImage|endswith:
      - '\lsass.exe'
      - '\services.exe'
    Image|endswith:
      - '\mimikatz.exe'
      - '\mimi.exe'
  condition: selection
  falsepositives:
    - Legitimate penetration testing
level: high
```

## Response Actions

1.  **Isolate Host**: Disconnect compromised host from network.
2.  **Reset Credentials**: Reset passwords for all compromised accounts.
3.  **Investigate**: Determine method of credential compromise and extent of access.
4.  **Remediate**: Remove malicious tools and fix vulnerabilities.

## MITRE ATT&CK Mapping

| Technique | Tactics | Detection |
|-----------|---------|-----------|
| Credential Dumping | Credential Access | LSASS process access, tool execution |

## References

- [MITRE ATT&CK T1003](https://attack.mitre.org/techniques/T1003/)
- [Credential Dumping Guide](https://example.com/credential-dumping-guide)

---
*MITRE ATT&CK: T1003 - OS Credential Dumping*
*Last Updated: 2026-01-29*
