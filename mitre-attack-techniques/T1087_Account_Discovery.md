# T1087: Account Discovery

## Summary

Account Discovery is a technique used by adversaries to gain knowledge about local system or domain accounts. This information helps them identify valid credentials, understand user privileges, and plan further actions for lateral movement or privilege escalation.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1087.001 | Local Account | Discovering local user accounts |
| T1087.002 | Domain Account | Discovering domain user accounts |
| T1087.003 | Email Account | Discovering email accounts |
| T1087.004 | Cloud Account | Discovering cloud-based accounts |

## How It's Used by Adversaries

- **Reconnaissance**: Gathering information about the environment.
- **Privilege Escalation**: Identifying privileged accounts.
- **Lateral Movement**: Finding accounts with access to other systems.

## What to Look For (Detection)

- Execution of commands like `net user`, `net group`, `query user`, `Get-ADUser`.
- Multiple failed login attempts (could indicate brute-force after discovery).
- Unusual queries against Active Directory (e.g., LDAP queries).
- Access to password managers or credential stores.

## Detection Rules

### Splunk Query - Local Account Discovery
```
index=windows (EventCode=4720 OR EventCode=4722 OR EventCode=4728)
| stats count by _time, EventCode, TargetUserName, SubjectUserName, ComputerName
| `security_content_annotations`
| fillnull value="-" _time
```

### Sigma Rule - Domain Account Discovery Commands
```yaml
title: Suspicious Account Discovery via Command Line
description: Detects common command-line utilities used for account discovery.
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    CommandLine|contains:
      - 'net user /domain'
      - 'net group /domain'
      - 'query user'
      - 'nltest /domain_trusts'
      - 'Get-ADUser'
  condition: selection
level: medium
```

## Response Actions

1.  **Review Logs**: Analyze security logs for account discovery commands.
2.  **Limit Access**: Restrict access to tools and commands used for discovery.
3.  **Educate Users**: Train users on phishing and social engineering.
4.  **Monitor**: Implement enhanced monitoring for suspicious account activities.

## MITRE ATT&CK Mapping

| Technique | Tactics | Detection |
|-----------|---------|-----------|
| Account Discovery | Discovery | Execution of discovery commands |

## References

- [MITRE ATT&CK T1087](https://attack.mitre.org/techniques/T1087/)
- [Account Discovery Best Practices](https://example.com/account-discovery-bp)

---
*MITRE ATT&CK: T1087 - Account Discovery*
*Last Updated: 2026-01-29*
