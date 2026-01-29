# T1486: Data Encrypted for Impact

## Summary

Adversaries may encrypt data on target systems to interrupt availability. This technique, commonly associated with ransomware, makes data inaccessible until a ransom is paid or other recovery methods are employed.

## How It's Used by Adversaries

- **Ransomware Deployment**: Encrypting files with strong encryption
- **Locker-style Attacks**: Locking users out of systems
- **File Deleting**: Deleting files to cause disruption
- **Disk Wiping**: Overwriting boot sectors or entire drives

## What to Look For (Detection)

### File Encryption Indicators
```
- Mass file modifications in short time window
- Files with new extensions (.encrypted, .lock, .ransom)
- Files becoming inaccessible
- High CPU/disk activity on file servers
```

### Process Activity
```
- Known ransomware processes running
- Suspicious PowerShell scripts
- Encrypted archive creation
- Shadow copies deletion
```

### Windows Event Indicators
```
- VSSadmin deletion (Event ID 4689)
- BCDedit modifications
- Service creation for encryption
```

## Detection Rules

### Splunk Query - File Encryption
```
index=windows EventCode=4656 
| search ObjectType=File ObjectName="*.encrypted" OR ObjectName="*.lock"
| stats count by Computer, User, ObjectName
```

### Splunk Query - Shadow Copy Deletion
```
index=windows EventID=4689 CommandLine="*vssadmin* delete *shadow*"
| stats count by Computer, CommandLine
```

### Sigma Rule - Ransomware Activity
```yaml
title: Ransomware File Activity
description: Detects file encryption activity
detection:
  selection:
    EventID: 4656
    ObjectName|contains:
      - '.encrypted'
      - '.lock'
      - '.crypt'
      - '.ransom'
  condition: selection
level: critical
```

## Response Actions

### Phase 1: Immediate Response
```
1. ISOLATE affected systems from network
2. DISABLE infected user accounts
3. BLOCK C2 communication if detected
4. PRESERVE volatile evidence (memory, logs)
```

### Phase 2: Assessment
```
- Identify scope of encryption
- Determine ransomware variant
- Check for backdoors/rersistence
- Assess backup integrity
```

### Phase 3: Recovery
```
- Restore from known-good backups
- Rebuild affected systems
- Reset all credentials
- Monitor for re-infection
```

### Phase 4: Post-Incident
```
- Conduct root cause analysis
- Update security controls
- Improve backup procedures
- Document lessons learned
```

## Common Ransomware Indicators

| Variant | Extension | Notable Characteristics |
|---------|-----------|------------------------|
| LockBit 3.0 | .lockbit | Ransomware-as-a-Service, fast encryption |
| Conti | .locked | Double extortion, leaks site |
| REvil | .encrypt, .locked | RaaS, high-profile targets |
| Ryuk | .RYK | Targeted, manual deployment |

## Prevention Controls

```
1. Backup & Recovery
   - Offline backups (air-gapped)
   - Regular backup testing
   - Versioned backups

2. Endpoint Protection
   - EDR with anti-ransomware
   - Application whitelisting
   - Behavior monitoring

3. Access Controls
   - Least privilege accounts
   - MFA for admin access
   - Network segmentation

4. Monitoring
   - File integrity monitoring
   - Behavior analytics
   - Threat intelligence feeds
```

## References

- [MITRE ATT&CK T1486](https://attack.mitre.org/techniques/T1486/)
- [No More Ransom Project](https://www.nomoreransom.org/)
- [CISA Ransomware Guide](https://www.cisa.gov/ransomware)

---
*MITRE ATT&CK: T1486 - Data Encrypted for Impact*
*Last Updated: 2026-01-28*

