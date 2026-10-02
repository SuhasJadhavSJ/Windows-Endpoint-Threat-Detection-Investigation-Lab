# Lab Architecture

## Purpose

This document describes the architecture of the Windows Endpoint Threat Detection & Investigation Lab.

## Current Architecture

```text
Host Machine
     |
     v
VirtualBox
     |
     v
Windows 10 Endpoint
     |
     +--> Windows Event Logs
     +--> Sysmon
     +--> PowerShell
     +--> Process Explorer
     +--> Procmon
     |
     v
Endpoint Investigation
     |
     +--> Event Analysis
     +--> Correlation
     +--> Timeline Reconstruction
     +--> IOC Extraction
     +--> MITRE ATT&CK Mapping
     |
     v
Wazuh Integration
(Later Phase)
```

## Current Environment

- Hypervisor: Oracle VirtualBox
- Endpoint: Windows 10 64-bit
- Memory: 3072 MB
- Processors: 2
- Virtual disk: 50 GB
- Network: NAT

## Investigation Data Flow

```text
Endpoint Activity
      |
      v
Telemetry Collection
      |
      v
Event Analysis
      |
      v
Correlation
      |
      v
Timeline
      |
      v
IOC Extraction
      |
      v
MITRE ATT&CK
      |
      v
Findings / Response Recommendations
```

## Future Wazuh Architecture

Wazuh will be integrated after the native Windows/Sysmon investigation phase:

```text
Windows Endpoint
      |
      v
Wazuh Agent
      |
      v
Wazuh Manager
      |
      v
Detection Rules
      |
      v
Alert
      |
      v
SOC Triage
      |
      v
Investigation
```

> This document describes the lab design. Actual investigation findings belong in the individual investigation cases.
