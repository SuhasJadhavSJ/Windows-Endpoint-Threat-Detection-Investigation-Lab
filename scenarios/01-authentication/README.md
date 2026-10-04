# Scenario 001: Network Authentication Attack

## Objective

Generate controlled failed and successful SMB authentication
activity against the Windows 10 lab VM.

## Attacker

Kali Linux host

IP: 192.168.56.1

## Target

Windows 10 VM

IP: 192.168.56.101

## Protocol

SMB
TCP/445

## Test Account

SOC-LabUser

## Attack Sequence

1. Five failed SMB authentication attempts
2. One successful SMB authentication

## Expected Telemetry

- Windows Security Event ID 4625
- Windows Security Event ID 4624

## Safety

This scenario was performed only against the author's
isolated Windows lab VM.