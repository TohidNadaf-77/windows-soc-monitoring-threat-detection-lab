# Windows SOC Monitoring & Threat Detection Lab

## IR-003 — Potential Windows Event Log Clearing

**Severity:** High  
**Status:** Closed — No Evidence of Clearing  
**Detection:** Potential Windows Event Log Clearing  
**Telemetry:** Sysmon Event ID 1 — Process Creation

### Initial Detection Search

The focused query for `wevtutil` event-log clearing commands returned **0 results**.

### Broader Investigation

A broader search found **226 `wevtutil.exe` executions**. The observed commands were primarily Microsoft Defender operations such as:

```text
install-manifest
uninstall-manifest
```

The observed parent process was `MsMpEng.exe`, under `NT AUTHORITY\SYSTEM`, with paths under Microsoft Defender platform directories.

### Detection Tuning

An initial broad pattern produced false positives because strings such as `MpClient.dll` contained the letters `cl`. The rule was refined to specifically match the command structure for:

```text
wevtutil.exe cl <logname>
wevtutil.exe clear-log <logname>
```

The refined query returned **0 results**.

### Verdict

**No evidence of Windows Event Log clearing was found.** The observed `wevtutil.exe` activity was consistent with legitimate Microsoft Defender manifest management.

---
