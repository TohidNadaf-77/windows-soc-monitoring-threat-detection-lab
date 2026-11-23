# Detection Use Cases

This document summarizes the three detection use cases validated in the lab. Queries should be reviewed and adjusted to the field names and event formats in the local Splunk deployment before reuse.

## 1. Encoded or download-related PowerShell

**Telemetry:** Sysmon Event ID 1 — Process Creation  
**Purpose:** Identify PowerShell command lines containing encoded-command or download-related indicators.  
**Validation:** A harmless encoded command was used: `Write-Host 'SOC lab test'`. The alert triggered as expected.  
**Analyst note:** Encoded PowerShell is a suspicious indicator, not proof of malicious activity. Decode and investigate the command and execution context.

## 2. Executable created in a Temp directory

**Telemetry:** Sysmon Event ID 11 — File Created  
**Purpose:** Identify executable files created under user or Windows Temp directories.  
**Validation:** A copy of Windows Notepad was placed in the user's Temp directory. The resulting file hash matched the source Notepad executable.  
**Analyst note:** A suspicious path alone does not establish malware. Correlate the creating process and verify the file where appropriate.

## 3. Potential Windows Event Log clearing

**Telemetry:** Sysmon Event ID 1 — Process Creation  
**Purpose:** Identify `wevtutil.exe` commands specifically associated with clearing an event log.  
**Validation:** No matching clearing activity was observed. Broader `wevtutil.exe` telemetry included Microsoft Defender manifest-management activity. The initial broad `cl` string pattern was refined because it matched unrelated text such as `MpClient.dll`.  
**Analyst note:** The refined query returned zero matches. Do not clear logs merely to generate a test event in a lab containing useful evidence.
