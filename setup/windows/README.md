# Windows Endpoint Setup

## Purpose

Document the Windows 10 endpoint used for the investigation lab.

## Environment

- Operating System: Windows 10 64-bit
- Virtualization: VirtualBox
- Memory: 3072 MB
- CPUs: 2
- Storage: 50 GB
- Network: NAT

## Evidence

Environment screenshots are stored under:

```text
screenshots/00-environment/
```

Current evidence:

- `01-virtualbox-overview.png`
- `02-virtualbox-system.png`
- `03-virtualbox-network.png`
- `04-windows-msinfo32.png`
- `05-windows-version.png`

## Baseline Checks

Before investigations, verify:

- Windows version
- System information
- Network configuration
- Event Viewer availability
- PowerShell availability
- Security log availability

## Status

- [x] Windows VM prepared
- [x] VirtualBox configuration documented
- [x] Windows system information documented
- [ ] Baseline event collection documented in detail
