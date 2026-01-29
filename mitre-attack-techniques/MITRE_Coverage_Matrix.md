# MITRE ATT&CK Coverage Matrix

This document provides an overview of MITRE ATT&CK techniques covered in this repository.

## Coverage Matrix

| ID | Technique | Document | Status | Detection Rules |
|----|-----------|----------|--------|-----------------|
| T1059.001 | PowerShell | T1059.001_PowerShell.md | ✅ Complete | ✅ Yes |
| T1110 | Brute Force | alert-triage-examples/brute-force-login.md | ✅ Complete | ✅ Yes |
| T1566 | Phishing | phishing_walkthrough_01.md | ✅ Complete | ⚠️ Basic |
| T1486 | Data Encrypted for Impact | playbook_ransomware.md | ✅ Complete | ⚠️ Basic |
| T1021 | Remote Services | (In Development) | 🔄 In Progress | ❌ No |
| T1003 | Credential Dumping | (Planned) | 📅 Planned | ❌ No |
| T1055 | Process Injection | (Planned) | 📅 Planned | ❌ No |
| T1082 | System Information Discovery | (Planned) | 📅 Planned | ❌ No |
| T1046 | Network Service Discovery | (Planned) | 📅 Planned | ❌ No |
| T1567 | Exfiltration Over Web Service | (Planned) | 📅 Planned | ❌ No |

## Tactics Coverage

| ATT&CK Tactic | Techniques Covered | Priority |
|---------------|-------------------|----------|
| Initial Access | T1566 (Phishing) | HIGH |
| Execution | T1059.001 (PowerShell) | HIGH |
| Persistence | (Planned) | MEDIUM |
| Privilege Escalation | T1055 (Planned) | MEDIUM |
| Defense Evasion | T1059.001 (Obfuscation) | HIGH |
| Credential Access | T1003 (Planned) | HIGH |
| Discovery | T1082, T1046 (Planned) | MEDIUM |
| Lateral Movement | T1021 (In Development) | HIGH |
| Collection | (Planned) | MEDIUM |
| Command and Control | (Planned) | HIGH |
| Exfiltration | T1567 (Planned) | MEDIUM |
| Impact | T1486 (Ransomware) | HIGH |

## Priority Techniques to Add

1. **T1021 - Remote Services**: RDP, SMB, VNC abuse
2. **T1003 - Credential Dumping**: LSASS, SAM database
3. **T1055 - Process Injection**: DLL injection, hollowing
4. **T1567 - Exfiltration Over Web Service**: Upload to cloud

## References

- [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/)
- [ATT&CK Technique Search](https://attack.mitre.org/techniques/)
- [APT Groups](https://attack.mitre.org/groups/)

---
*Last Updated: 2026-01-28*
*Coverage Goal: 20+ techniques by Q2 2026*

