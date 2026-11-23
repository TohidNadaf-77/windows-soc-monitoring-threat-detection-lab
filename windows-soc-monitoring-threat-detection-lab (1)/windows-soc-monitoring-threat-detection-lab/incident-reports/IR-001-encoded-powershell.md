# Windows SOC Monitoring & Threat Detection Lab

## IR-001 — Encoded PowerShell Execution

**Severity:** Medium  
**Status:** Closed — Benign / Authorized Lab Test  
**Detection:** Suspicious PowerShell Execution - Encoded or Download  
**Telemetry:** Sysmon Event ID 1 — Process Creation

### Alert Summary

Splunk detected a PowerShell process containing `-NoProfile -EncodedCommand`.

### Process Chain

```text
explorer.exe
    ↓
powershell.exe (PID 9008)
    ↓
powershell.exe (PID 9804)
    └── -NoProfile -EncodedCommand
```

### Investigation

The Base64 payload was decoded and produced:

```text
Write-Host 'SOC lab test'
```

The command only displayed text. No download, persistence, file modification, or event-log clearing behavior was identified.

### MITRE ATT&CK Mapping

- **T1059.001 — PowerShell**
- **T1027 — Obfuscated Files or Information**

### Verdict

**Benign / Authorized Security Test.** The detection worked as intended and identified behavior that warranted investigation.

---
