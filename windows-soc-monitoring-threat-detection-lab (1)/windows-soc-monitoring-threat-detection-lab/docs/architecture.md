# Lab Architecture

## Data flow

```text
Windows 11 endpoint
        |
        v
      Sysmon
        |
        v
Splunk Universal Forwarder
        |  TCP 9997
        v
Splunk Enterprise
        |
        +--> SPL searches and detection queries
        +--> Scheduled alerts
        +--> SOC monitoring dashboard
```

## Components

- **Windows 11:** Endpoint used for controlled lab activity and telemetry generation.
- **Sysmon:** Records selected endpoint events, including process creation (Event ID 1) and file creation (Event ID 11).
- **Splunk Universal Forwarder:** Collects the Sysmon Operational event channel and forwards events to Splunk Enterprise.
- **Splunk Enterprise:** Receives, searches, correlates, alerts on, and visualizes the telemetry.

## Lab network note

The forwarder-to-receiver connection used TCP port 9997. The lab was hosted in VirtualBox. Do not publish private IP addresses, credentials, tokens, or unredacted environment details in screenshots or configuration files.
