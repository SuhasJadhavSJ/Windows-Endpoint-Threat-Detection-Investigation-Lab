# Sysmon Setup

## Purpose

Document Sysmon deployment, configuration, validation, and telemetry used by the investigation lab.

## Why Sysmon Is Used

Sysmon provides additional endpoint telemetry for investigating:

- Process creation
- Network connections
- File creation
- Registry activity
- DNS queries
- Other endpoint behaviors supported by the configured Sysmon version/configuration

## Installation

Installation steps will be documented here after Sysmon is deployed in the Windows VM.

## Configuration

The final Sysmon configuration used by this lab will be recorded here.

> Do not claim that a Sysmon event is available in an investigation unless the endpoint actually generated and retained that telemetry.

## Validation

Validation will include:

```powershell
Get-Service Sysmon*
```

and review of:

```text
Applications and Services Logs
└── Microsoft
    └── Windows
        └── Sysmon
            └── Operational
```

## Evidence

Setup screenshots will be stored under:

```text
screenshots/01-lab-setup/
```

## Status

- [ ] Sysmon installed
- [ ] Sysmon service verified
- [ ] Sysmon Operational log verified
- [ ] Telemetry validated
- [ ] Configuration documented
