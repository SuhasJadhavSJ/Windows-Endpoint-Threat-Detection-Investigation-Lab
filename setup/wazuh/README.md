# Wazuh Setup

## Purpose

Document the later integration of the Windows endpoint with Wazuh.

## Planned Architecture

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
Alerts
      |
      v
SOC Triage
```

## Current Status

Wazuh is intentionally a later phase of this project. The initial investigations focus on understanding native Windows and Sysmon telemetry first.

- [ ] Wazuh agent deployment
- [ ] Manager connectivity
- [ ] Windows log collection
- [ ] Alert validation
- [ ] Custom detection rules
- [ ] Detection tuning
