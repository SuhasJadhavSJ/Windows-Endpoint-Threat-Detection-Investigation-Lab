# Extracted IOCs

This document is the central IOC index for completed investigations.

## IOC Handling Principle

An indicator should only be recorded when it is actually supported by investigation evidence.

## IOC Table

| Investigation | IOC Type | Indicator | Evidence Source | Confidence | Notes |
|---|---|---|---|---|---|
| 001 | IP address | `192.168.56.1` | Windows Security Event IDs 4625 and 4624 | High | Source address for the controlled SMB authentication activity from the Kali lab host. This is a private lab address, not a malicious public IP. |
| 001 | User account | `SOC-LabUser` | Windows Security Event IDs 4625 and 4624 | High | Account targeted by five failed network authentication attempts and subsequently authenticated successfully. |
| 001 | IP address | `192.168.56.101` | Lab network configuration and investigation scope | High | Windows 10 lab target. This is the destination system, not an attacker IOC. |
| 001 | Service / Protocol | `SMB / TCP 445` | Controlled attack execution and successful SMB share enumeration | High | Network service used during the authentication investigation. Recorded as investigation context rather than a malicious IOC. |

## Investigation 001 Context

The authentication investigation generated five failed SMB authentication attempts followed by one successful authentication.

Timeline:

- `17:44:18` - Failed
- `17:45:01` - Failed
- `17:45:04` - Failed
- `17:45:07` - Failed
- `17:45:11` - Failed
- `17:45:25` - Successful

The failed authentications were recorded as Windows Security Event ID 4625. The successful authentication was recorded as Event ID 4624.

The successful event recorded:

- Target account: `SOC-LabUser`
- Logon Type: `3` (Network)
- Source IP: `192.168.56.1`
- Target system: `192.168.56.101`
- Successful Logon ID: `0x34a6b4`

## IOC Classification Notes

`192.168.56.1` is the Kali host on the isolated VirtualBox host-only network. It should not be presented as a malicious external indicator.

`192.168.56.101` is the Windows 10 investigation target and is therefore documented as infrastructure context rather than an attacker IOC.

`SOC-LabUser` is a lab account used to generate controlled authentication telemetry. It is not a malicious account.

`SMB/TCP 445` identifies the service involved in the investigation and is not itself evidence of malicious activity.

No domain, URL, file hash, malicious filename, registry key, malicious process, or command-line IOC was established during Investigation 001.

## IOC Types

Potential IOC categories include:

- IP address
- Domain
- URL
- File hash
- File name
- File path
- Registry key
- User account
- Process
- Command line

> Placeholder values must not be presented as actual findings.
