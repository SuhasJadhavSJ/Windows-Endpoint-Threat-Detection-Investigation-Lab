# MITRE ATT&CK Mapping

This document provides the central index of MITRE ATT&CK techniques supported by completed investigations in the Windows Endpoint Threat Detection & Investigation Lab.

## Mapping Principle

A technique is mapped only when the observed behavior and available investigation evidence support the mapping. Techniques that have not yet been demonstrated are listed as future investigation targets and are not treated as confirmed findings.

## Central Mapping Table

| Investigation | Observed Behavior | Technique | Evidence | Confidence |
|---|---|---|---|---|
| Investigation 001 - Network Authentication Attack | Five failed network authentication attempts against `SOC-LabUser`, followed by a successful network authentication from the same source IP | T1110 - Brute Force | Windows Security Event ID 4625 ×5, followed by Event ID 4624; Logon Type 3; source IP `192.168.56.1`; target account `SOC-LabUser`; successful authentication at `17:45:25` | High |

## Investigation 001 Evidence Summary

### Technique: T1110 - Brute Force

The controlled investigation generated repeated authentication failures against the same Windows account, followed by a successful authentication from the same source. This behavior supports mapping the activity to the MITRE ATT&CK **Brute Force (T1110)** technique.

Observed evidence:

- Target account: `SOC-LabUser`
- Source IP: `192.168.56.1`
- Target Windows VM: `192.168.56.101`
- Protocol: SMB
- Destination port: TCP/445
- Failed authentication events: 5 × Windows Security Event ID 4625
- Successful authentication event: Windows Security Event ID 4624
- Logon Type: 3 (Network)
- Successful Logon ID: `0x34a6b4`
- Successful authentication time: `04-10-2026 17:45:25`

### Investigation Limitation

This investigation demonstrated authentication behavior only. No post-authentication process execution, persistence, privilege escalation, lateral movement, or data exfiltration was performed or confirmed. Therefore, those behaviors are not mapped as findings for Investigation 001.

## Techniques Not Yet Confirmed

The following techniques remain investigation targets for future scenarios. They must not be treated as confirmed findings until corresponding telemetry and evidence are collected.

- **T1059.001 - Command and Scripting Interpreter: PowerShell**
- **T1059.003 - Command and Scripting Interpreter: Windows Command Shell**
- **T1053.005 - Scheduled Task/Job: Scheduled Task**
- **T1543.003 - Create or Modify System Process: Windows Service**
- **T1547.001 - Boot or Logon Autostart Execution: Registry Run Keys / Startup Folder**
- **T1036 - Masquerading**

## Mapping Status

| Technique | Status |
|---|---|
| T1110 - Brute Force | Confirmed in Investigation 001 |
| T1059.001 - PowerShell | Not yet investigated |
| T1059.003 - Windows Command Shell | Not yet investigated |
| T1053.005 - Scheduled Task | Not yet investigated |
| T1543.003 - Windows Service | Not yet investigated |
| T1547.001 - Registry Run Keys / Startup Folder | Not yet investigated |
| T1036 - Masquerading | Not yet investigated |
