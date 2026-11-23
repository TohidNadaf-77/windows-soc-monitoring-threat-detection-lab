# Windows SOC Monitoring & Threat Detection Lab

## IR-002 — Executable Created in Temporary Directory

**Severity:** Medium  
**Status:** Closed — Benign / Authorized Lab Test  
**Detection:** Executable Created in Temp Directory  
**Telemetry:** Sysmon Event ID 11 — File Created

### Alert Summary

Splunk detected creation of:

```text
C:\Users\SOC\AppData\Local\Temp\SOC-Lab-Test2.exe
```

### Process Correlation

The Sysmon Event ID 11 record identified PowerShell as the creating process. The matching ProcessGuid correlated the file creation to:

```text
explorer.exe
    ↓
powershell.exe (PID 12956)
    ↓
SOC-Lab-Test2.exe
```

### File Verification

The SHA-256 hash of the created file matched the hash of the original Windows Notepad executable:

```text
C:\Windows\System32\notepad.exe
SHA256: 3F543719A819A9769D8F138A0254...

C:\Users\SOC\AppData\Local\Temp\SOC-Lab-Test2.exe
SHA256: 3F543719A819A9769D8F138A0254...
```

The full hash was observed in the lab; only a shortened representation is included here.

### Verdict

**Benign / Authorized Security Test.** The detection identified an executable created in a Temp directory, but investigation showed it was an identical copy of legitimate Windows Notepad.

---
