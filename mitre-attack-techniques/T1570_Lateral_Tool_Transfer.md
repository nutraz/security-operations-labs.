# T1570: Lateral Tool Transfer

## Summary

Lateral Tool Transfer involves an adversary transferring tools or files from an external system to a compromised host or between compromised hosts within a network. This enables them to deploy additional capabilities, such as custom malware, reconnaissance tools, or credential access utilities, to further their objectives.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1570.001 | Remote Desktop Protocol (RDP) | Transferring files via RDP share |
| T1570.002 | SMB/Windows Admin Shares | Copying tools to ADMIN$ or C$ |
| T1570.003 | FTP/SFTP | Using file transfer protocols |
| T1570.004 | Web Services | Downloading via HTTP/HTTPS |
| T1570.005 | Email | Sending tools via email |

## How It's Used by Adversaries

- **Deployment**: Placing new tools on target systems.
- **Defense Evasion**: Using native or legitimate mechanisms for transfer.
- **Establishing Foothold**: Bringing in additional capabilities.

## What to Look For (Detection)

- Unusual file transfers to/from systems.
- Execution of `scp`, `sftp`, `wget`, `curl`, `bitsadmin` for downloads.
- Files created in unusual directories (e.g., `C:\Windows\Temp`).
- Tools observed on systems that typically do not have them (e.g., penetration testing tools).

## Detection Rules

### Splunk Query - Unusual File Creation
```
index=windows EventCode=11
| stats count by _time, host, Image, TargetFilename, User
| where like(TargetFilename, "%Temp%\\%.exe") OR like(TargetFilename, "%Public%\\%.exe")
| `security_content_annotations`
| fillnull value="-" _time
```

### Sigma Rule - Bitsadmin Download
```yaml
title: Bitsadmin Download Activity
description: Detects suspicious file downloads using bitsadmin.
logsource:
  category: process_creation
  product: windows
detection:
  selection:
    Image|endswith: '\bitsadmin.exe'
    CommandLine|contains: 'transfer'
  condition: selection
level: medium
```

## Response Actions

1.  **Contain**: Isolate systems involved in tool transfer.
2.  **Identify Origin**: Determine the source of the transferred tools.
3.  **Analyze Tools**: Reverse engineer or analyze transferred tools for capabilities.
4.  **Block**: Prevent further transfers by blocking relevant protocols or sources.

## MITRE ATT&CK Mapping

| Technique | Tactics | Detection |
|-----------|---------|-----------|
| Lateral Tool Transfer | Lateral Movement | Unusual file transfers, execution of transfer tools |

## References

- [MITRE ATT&CK T1570](https://attack.mitre.org/techniques/T1570/)
- [Detecting Lateral Tool Transfer](https://example.com/detecting-ltt)

---
*MITRE ATT&CK: T1570 - Lateral Tool Transfer*
*Last Updated: 2026-01-29*
