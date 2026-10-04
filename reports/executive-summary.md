# Executive Summary

## Project

Windows Endpoint Threat Detection & Investigation Lab

## Purpose

Summarize the completed endpoint investigations and the security lessons demonstrated by the project.

## Current Status

The Windows 10 endpoint investigation lab has been prepared and documented. Sysmon telemetry was validated, a controlled host-only investigation network was established, and the first endpoint investigation has been completed.

The current lab architecture uses Kali Linux as the controlled source system and a Windows 10 virtual machine as the investigation target.

## Investigations Completed

| Case | Investigation | Status | Key Finding |
|---|---|---|---|
| 001 | Network Authentication Attack | Completed | Five failed SMB network authentication attempts against `SOC-LabUser` were followed by one successful Type 3 authentication from the same source IP. |
| 002 | PowerShell | Not started | TBD |
| 003 | File Execution | Not started | TBD |

## Investigation 001: Network Authentication Attack

The first investigation simulated a controlled SMB authentication attack against the Windows 10 lab endpoint.

### Observed Activity

- Source: Kali Linux host, `192.168.56.1`
- Target: Windows 10 VM, `192.168.56.101`
- Protocol: SMB over TCP/445
- Target account: `SOC-LabUser`
- Failed authentications: 5
- Successful authentication: 1
- Windows Security Event ID: 4625 for failed authentication
- Windows Security Event ID: 4624 for successful authentication
- Logon Type: 3 (Network)
- Successful Logon ID: `0x34a6b4`

### Timeline

| Time | Event | Result |
|---|---|---|
| 17:44:18 | 4625 | Failed authentication |
| 17:45:01 | 4625 | Failed authentication |
| 17:45:04 | 4625 | Failed authentication |
| 17:45:07 | 4625 | Failed authentication |
| 17:45:11 | 4625 | Failed authentication |
| 17:45:25 | 4624 | Successful authentication |

### Finding

The evidence demonstrates a repeated network authentication failure pattern against a single account followed by a successful authentication from the same controlled source. The activity is consistent with a password-guessing scenario performed in the authorized lab environment.

No post-authentication process execution, persistence, privilege escalation, or data exfiltration was performed or established during this investigation. The investigation therefore does not claim compromise beyond the successful authentication event.

## Overall Findings

The completed investigation demonstrated the value of correlating multiple Windows Security events instead of treating an individual authentication failure as proof of compromise.

The primary findings from Investigation 001 were:

- Repeated Event ID 4625 failures can indicate password-guessing activity when correlated by account, source, and time.
- Event ID 4624 can establish that authentication subsequently succeeded.
- Logon Type 3 provided evidence of network-based authentication consistent with the SMB test.
- Source IP correlation linked the failed and successful authentication activity to the controlled Kali host.
- Timeline reconstruction provided stronger evidence than examining individual events in isolation.
- Investigation conclusions must remain limited to the telemetry actually generated and analyzed.

## MITRE ATT&CK Mapping

Investigation 001 was mapped to **T1110, Brute Force**, based on the repeated authentication failures followed by successful authentication in the controlled scenario.

Other techniques listed in the repository remain investigation targets and are not confirmed findings until corresponding evidence is generated and analyzed.

## Detection Improvements

Based on Investigation 001, future detection engineering should focus on identifying repeated Windows authentication failures and correlating them with successful authentication from the same source.

Potential detection improvements include:

- Correlating multiple Event ID 4625 events by source IP and target account.
- Increasing severity when a successful Event ID 4624 follows repeated failures within a defined time window.
- Including Logon Type and source IP in alert context.
- Mapping confirmed authentication detections to MITRE ATT&CK.
- Validating the resulting detection logic against controlled lab activity before treating it as production-ready.

Wazuh detection rules have not yet been treated as completed findings for this investigation unless they are separately implemented and validated.

## Limitations

This project is an authorized educational home lab. The Investigation 001 findings are limited to the Windows Security telemetry generated during the controlled SMB authentication test.

The successful authentication demonstrates successful credential validation, but by itself does not demonstrate malicious post-authentication activity or system compromise.

## Conclusion

Investigation 001 successfully demonstrated an evidence-driven Windows authentication investigation from controlled attack generation through event analysis, timeline reconstruction, IOC extraction, and MITRE ATT&CK mapping.

The next investigations will expand the project into PowerShell activity and file execution, followed by additional endpoint investigation and detection engineering work.
