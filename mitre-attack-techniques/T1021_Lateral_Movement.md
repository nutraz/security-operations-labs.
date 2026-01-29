# T1021: Lateral Movement

## Summary

Lateral movement refers to techniques that adversaries use to move through a network, accessing systems and data after initially compromising an initial target. This allows attackers to expand their foothold and reach critical assets.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1021.001 | Remote Desktop Protocol (RDP) | Using RDP to access remote systems |
| T1021.002 | SMB/Windows Admin Shares | Using SMB to access shared files/folders |
| T1021.003 | Remote Process Execution via WMI | Using WMI to execute processes remotely |
| T1021.004 | SSH | Using SSH to access Unix/Linux systems |
| T1021.005 | VNC | Using Virtual Network Computing |
| T1021.006 | Windows Remote Management | Using WinRM for remote management |

## How It's Used by Adversaries

- **RDP Hijacking**: Taking over existing RDP sessions
- **Pass-the-Hash**: Authenticating using NTLM hashes
- **Pass-the-Ticket**: Using Kerberos tickets for authentication
- **SMB Exploitation**: Using EternalBlue or similar exploits
- **WMI Exec**: Executing remote processes via WMI

## What to Look For (Detection)

### RDP Activity
```
- Multiple RDP connections from single source
- Logins outside business hours
- RDP from internal to sensitive systems
- Concurrent RDP sessions
```

### SMB Activity
```
- Large SMB data transfers
- Access to Admin$ shares
- SMB from workstation to workstation
- Lateral movement to file servers
```

### WMI Activity
```
- WMIExec, PSExec usage
- Remote process creation
- Scheduled task creation
- CIM subscriptions
```

## Detection Rules

### Splunk Query - RDP Lateral Movement
```
index=windows EventCode=4624 LogonType=10
| stats count by SrcIP, TargetUser, TargetHost
| where count > 5
| sort -count
```

### Splunk Query - SMB Lateral Movement
```
index=windows EventID=5140 ShareName="\\*\\ADMIN$"
| stats count by SourceAddress, ShareName, TargetHost
| where count > 3
```

### Sigma Rule - Remote WMI Execution
```yaml
title: WMI Remote Process Execution
description: Detects WMI execution for lateral movement
detection:
  selection:
    EventID: 4688
    ParentCommandLine: "*wmiprvse.exe"
    NewCommandLine|contains:
      - 'powershell'
      - 'cmd.exe'
  condition: selection
level: high
```

## Response Actions

1. **Immediate Containment**
   - Isolate compromised systems
   - Disable lateral movement paths
   - Block RDP/SMB between segments

2. **Investigation**
   - Correlate logon activity
   - Identify compromised credentials
   - Map attacker's path

3. **Remediation**
   - Reset passwords
   - Revoke sessions
   - Rebuild compromised systems

4. **Prevention**
   - Network segmentation
   - Limit admin privileges
   - Enable NLA for RDP
   - Implement MFA for RDP

## MITRE ATT&CK Mapping

| Technique | Tactics | Detection |
|-----------|---------|-----------|
| RDP | Lateral Movement | EventID 4624, LogonType=10 |
| SMB | Lateral Movement | EventID 5140, Admin$ access |
| WMI | Execution, Lateral Movement | EventID 4688, Parent=wmiprvse |
| PSExec | Execution, Lateral Movement | Service creation, EventID 7045 |

## References

- [MITRE ATT&CK T1021](https://attack.mitre.org/techniques/T1021/)
- [Lateral Movement Detection Guide](https://www.redcanary.com/blog/lateral-movement/)

---
*MITRE ATT&CK: T1021 - Remote Services*
*Last Updated: 2026-01-28*

