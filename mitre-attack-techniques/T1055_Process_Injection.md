# T1055: Process Injection

## Summary

Process Injection is a technique used by adversaries to inject code into another process to evade defenses, elevate privileges, or maintain persistence. This allows the malicious code to run under the context of a legitimate process, making it harder to detect.

## Sub-techniques

| ID | Sub-technique | Description |
|----|---------------|-------------|
| T1055.001 | Dynamic-link Library Injection | Injecting a DLL into a process |
| T1055.002 | Portable Executable Injection | Injecting a PE into a process |
| T1055.003 | Thread Execution Hijacking | Hijacking an existing thread |
| T1055.004 | Asynchronous Procedure Call | Using APC for code injection |
| T1055.005 | Thread Local Storage | Using TLS for injection |
| T1055.008 | Ptrace System Call | Injecting code using ptrace (Linux) |

## How It's Used by Adversaries

- **Defense Evasion**: Running malicious code in a trusted process.
- **Privilege Escalation**: Injecting into higher-privileged processes.
- **Persistence**: Maintaining execution on a system.

## What to Look For (Detection)

- Unusual memory allocations (e.g., `VirtualAllocEx`, `NtCreateSection`).
- Creation of remote threads in other processes (`CreateRemoteThread`).
- Modification of process memory protections (`VirtualProtectEx`).
- Processes exhibiting abnormal behavior for their type.

## Detection Rules

### Splunk Query - Remote Thread Creation
```
index=windows EventCode=8
| stats count by _time, host, ProcessName, TargetProcessName, TargetProcessId
| `security_content_annotations`
| fillnull value="-" _time
```

### Sigma Rule - DLL Injection via CreateRemoteThread
```yaml
title: Remote Thread Creation via CreateRemoteThread
description: Detects remote thread creation, often used for process injection.
logsource:
  product: windows
  service: sysmon
  definition: 'filter on event ID 8 (CreateRemoteThread)'
detection:
  selection:
    EventID: 8
    TargetImage|endswith:
      - '\explorer.exe'
      - '\lsass.exe'
      - '\winlogon.exe'
  condition: selection
  falsepositives:
    - Legitimate software (e.g., AV, debugging tools)
level: high
```

## Response Actions

1.  **Isolate Host**: Isolate the affected system from the network.
2.  **Terminate Malicious Process**: Identify and terminate the injected process/thread.
3.  **Analyze**: Perform memory forensics to understand the injection technique and payload.
4.  **Remediate**: Remove persistence mechanisms and patch vulnerabilities.

## MITRE ATT&CK Mapping

| Technique | Tactics | Detection |
|-----------|---------|-----------|
| Process Injection | Defense Evasion | Remote thread creation, abnormal memory regions |

## References

- [MITRE ATT&CK T1055](https://attack.mitre.org/techniques/T1055/)
- [Process Injection Techniques](https://example.com/process-injection-techniques)

---
*MITRE ATT&CK: T1055 - Process Injection*
*Last Updated: 2026-01-29*
