# Windows SOC Monitoring & Threat Detection Lab

A hands-on SOC/SIEM lab built with **Windows 11, Sysmon, Splunk Enterprise, and Splunk Universal Forwarder** to practice endpoint telemetry collection, detection engineering, alerting, investigation, and false-positive tuning.

## Project Overview

The lab simulates an endpoint sending Sysmon telemetry to Splunk. SPL detections are used to identify suspicious PowerShell behavior, executable creation in temporary directories, and potential Windows event-log clearing.

## Lab Architecture

The following diagram illustrates the end-to-end data flow of the Windows SOC monitoring lab, from endpoint telemetry collection to SIEM monitoring and detection.

![Windows SOC Lab Architecture](docs/lab-architecture.png)

### Data Flow

```text
Windows 11 Endpoint
        |
        v
      Sysmon
        |
        v
Splunk Universal Forwarder
        | TCP 9997
        v
Splunk Enterprise
        |
        +--> SPL Searches and Detection Queries
        +--> Scheduled Alerts
```

## Technologies

- Windows 11
- Sysmon
- Splunk Enterprise
- Splunk Universal Forwarder
- SPL
- PowerShell
- VirtualBox

## Detection Use Cases

### 1. Encoded PowerShell

Detects PowerShell activity containing encoded-command or download-related patterns.

**Outcome:** Successfully validated with a harmless controlled test.

### 2. Executable Created in Temp Directory

Detects `.exe` files created in Windows or user Temp directories.

**Outcome:** Successfully validated with a controlled test using a copy of Windows Notepad.

### 3. Potential Windows Event Log Clearing

Detects `wevtutil.exe` commands specifically associated with event-log clearing.

**Outcome:** No clearing activity observed. Detection was tuned to remove false positives from legitimate Microsoft Defender `wevtutil` activity.

## SOC Dashboard

The Splunk dashboard includes:

- Total Sysmon Events
- Sysmon Events by Event ID
- PowerShell Events
- Suspicious PowerShell Events
- Temp Directory Executables
- Potential Event Log Clearing
- PowerShell Activity by User
- Top Process Executions
- Recent Suspicious Activity
- Detection Summary

## Investigation Cases

### IR-001 — Encoded PowerShell Execution

The alert detected encoded PowerShell. Investigation reconstructed the process chain and decoded the payload to `Write-Host 'SOC lab test'`. The activity was classified as a benign authorized test.

**MITRE ATT&CK:** T1059.001, T1027

### IR-002 — Executable Created in Temp Directory

Sysmon Event ID 11 identified an executable created under the user's Temp directory. Process correlation linked the event to PowerShell, and SHA-256 comparison confirmed the file was identical to the legitimate Windows Notepad executable. The activity was classified as a benign authorized test.

### IR-003 — Potential Windows Event Log Clearing

The focused clearing detection returned zero results. A broader search found legitimate Microsoft Defender `wevtutil.exe` activity. The detection logic was refined after false positives caused by a broad `cl` pattern. The refined detection returned zero results.

## Key SOC Skills Demonstrated

- SIEM deployment and log ingestion
- Sysmon telemetry analysis
- SPL query development
- Detection engineering
- Scheduled alert creation
- Process-tree and ProcessGuid correlation
- File/hash verification
- False-positive investigation and rule tuning
- MITRE ATT&CK mapping
- Incident documentation

## Resume Project Entry

**Windows SOC Monitoring & Threat Detection Lab | Splunk, Sysmon, Windows 11**

- Built a Windows SOC lab using Splunk, Universal Forwarder, and Sysmon for security monitoring.
- Created detections for encoded PowerShell, Temp executable creation, and event-log clearing.
- Configured alerts and built a Splunk dashboard for monitoring suspicious activity.

## Project Status

Core lab setup, telemetry collection, detections, alerts, dashboard, and three controlled investigation cases are complete. Further improvements can include additional detection use cases, more realistic attack simulations, and automated investigation workflows.
