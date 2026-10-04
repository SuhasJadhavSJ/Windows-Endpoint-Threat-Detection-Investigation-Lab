# Incident Report: SMB Authentication Attack

**Incident ID:** INC-001  
**Date:** 04 October 2026  
**Environment:** Windows 10 VM / Kali Linux host  
**Investigation Type:** Controlled Blue Team Detection & Investigation Lab  
**Status:** Closed - Controlled Simulation  
**Severity:** Medium  
**Detection Source:** Windows Security Event Log  
**Primary Event IDs:** 4625, 4624  

---

## 1. Executive Summary

A controlled network authentication attack was performed against a Windows 10 virtual machine from the Kali Linux host. The objective was to simulate repeated password-guessing activity and investigate the resulting Windows authentication telemetry.

Five consecutive failed SMB authentication attempts were generated against the local Windows account `SOC-LabUser`. The failures occurred at 17:44:18, 17:45:01, 17:45:04, 17:45:07, and 17:45:11 on 04 October 2026. A successful authentication using the same account followed at 17:45:25.

The failed authentication was recorded as Windows Security Event ID 4625 with Logon Type 3 (Network). The successful authentication was recorded as Event ID 4624, also with Logon Type 3. The successful event identified the source IP as `192.168.56.1`, which is the Kali host on the isolated VirtualBox Host-only network. The Windows target used `192.168.56.101`.

No post-authentication execution, lateral movement, privilege escalation, persistence, or destructive activity was performed or investigated during this scenario. The successful authentication therefore represents confirmed credential acceptance, but not confirmed compromise beyond authentication.

---

## 2. Objective

The investigation was designed to demonstrate the complete workflow for a Windows network authentication incident:

```text
Controlled Attack
      ↓
Windows Security Telemetry
      ↓
Event ID 4625 / 4624
      ↓
Account + Source IP + Logon Type Correlation
      ↓
Timeline Reconstruction
      ↓
IOC / Evidence Extraction
      ↓
MITRE ATT&CK Mapping
      ↓
Impact Assessment
      ↓
Detection Improvement
```

---

## 3. Lab Architecture

```text
Kali Linux Host
192.168.56.1
      |
      | VirtualBox Host-only Network
      | SMB / TCP 445
      v
Windows 10 VM
192.168.56.101
      |
      +-- Windows Security Event Log
      +-- Sysmon
      +-- SOC-LabUser
```

The SMB service was verified to be listening on TCP port 445. Windows Firewall was configured to allow the required SMB inbound rule while leaving the firewall enabled.

---

## 4. Attack Simulation

The attack was performed only against the self-owned Windows 10 lab VM.

### Attack method

SMB authentication was initiated from Kali using `smbclient` against:

```text
Target: 192.168.56.101
Protocol: SMB
Destination Port: TCP/445
Account: SOC-LabUser
```

Five attempts used an intentionally incorrect password. The sixth authentication used the correct password.

### Observed Kali-side result

The five failed attempts returned:

```text
NT_STATUS_LOGON_FAILURE
```

The successful authentication returned the available SMB shares, including:

```text
ADMIN$
C$
IPC$
```

A later SMB1 workgroup-enumeration timeout occurred after authentication. This was not treated as another authentication failure and was outside the primary investigation objective.

---

## 5. Authentication Evidence

### 5.1 Failed Authentication - Event ID 4625

Observed values from the investigated 4625 event:

| Field | Observed Value |
|---|---|
| Event ID | 4625 |
| TargetUserName | `SOC-LabUser` |
| SubjectLogonId | `0x0` |
| Status | `0xc000006d` |
| FailureReason | `%2313` |
| SubStatus | `0xc000006a` |
| LogonType | `3 (Network logon)` |
| Source Network Address | `192.168.56.1` |

The evidence indicates a failed network authentication attempt against `SOC-LabUser` from the Kali host.

The `0xc000006a` substatus is consistent with an incorrect password for this controlled test.

### 5.2 Successful Authentication - Event ID 4624

Observed values from the successful 4624 event:

| Field | Observed Value |
|---|---|
| Event ID | 4624 |
| TargetUserName | `SOC-LabUser` |
| TargetUserSid | `S-1-5-21-...-1001` |
| TargetLogonId | `0x34a6b4` |
| LogonType | `3 (Network)` |
| WorkstationName | `SUHAS` |
| IpAddress | `192.168.56.1` |
| IpPort | `56392` |
| Time | `04-10-2026 17:45:25` |

The successful event establishes that the same source IP successfully authenticated as the targeted account using a network logon.

---

## 6. Timeline

| Time | Event | Evidence | Interpretation |
|---|---|---|---|
| 17:44:18 | Failed authentication | 4625 | First failed SMB authentication |
| 17:45:01 | Failed authentication | 4625 | Repeated failure against same account |
| 17:45:04 | Failed authentication | 4625 | Repeated failure |
| 17:45:07 | Failed authentication | 4625 | Repeated failure |
| 17:45:11 | Failed authentication | 4625 | Fifth failed authentication |
| 17:45:25 | Successful authentication | 4624 | Successful network authentication from same source |

### Timeline assessment

The five failed attempts were followed by a successful authentication approximately 14 seconds after the final failure. This sequence is consistent with a password-guessing pattern in the controlled lab scenario.

The investigation does not establish that the successful session performed additional malicious actions after authentication because no post-authentication activity was generated or analyzed.

---

## 7. Correlation Analysis

The strongest correlation points are:

1. **Same account:** `SOC-LabUser`
2. **Same source:** `192.168.56.1`
3. **Same authentication type:** Logon Type 3
4. **Same target:** Windows 10 VM at `192.168.56.101`
5. **Temporal relationship:** five failures followed by success
6. **Protocol context:** SMB over TCP/445

The successful event's `TargetLogonId` of `0x34a6b4` identifies the newly established successful logon session. It should not be treated as the identifier for the earlier failed attempts, because failed authentication attempts do not establish the same authenticated session.

---

## 8. IOC / Evidence Extraction

### Network evidence

| Type | Value | Classification |
|---|---|---|
| Source IP | `192.168.56.1` | Lab attacker/source address |
| Target IP | `192.168.56.101` | Lab Windows endpoint |
| Destination Port | `445/TCP` | SMB service |
| Source Port | `56392` | Observed on successful 4624 |

### Account evidence

| Type | Value | Classification |
|---|---|---|
| Target account | `SOC-LabUser` | Lab account |
| Successful Logon ID | `0x34a6b4` | Successful session identifier |

**Important:** `192.168.56.1` is an IOC/evidence artifact for this lab scenario. It must not be described as a malicious external IP because it is the controlled Kali host address on the isolated network.

---

## 9. MITRE ATT&CK Mapping

### T1110.001 - Password Guessing

**Mapping:** Confirmed for the controlled simulation.

The scenario deliberately generated repeated authentication attempts using an incorrect password followed by a successful authentication. This models password-guessing behavior.

### SMB / Windows network service

SMB over TCP/445 was the network service used during the authentication activity. The successful authentication demonstrated access to the SMB service and share enumeration.

No remote command execution, lateral movement, or administrative share access was performed after authentication. Therefore, this report does **not** claim confirmed post-authentication lateral movement.

---

## 10. Impact Assessment

### Confirmed

- Five failed authentication attempts occurred.
- The targeted account was `SOC-LabUser`.
- The source was the controlled Kali host at `192.168.56.1`.
- A successful network authentication occurred at 17:45:25.
- SMB shares were returned after successful authentication.

### Not demonstrated

- Remote code execution
- Privilege escalation
- Persistence
- Credential dumping
- Malware execution
- File modification
- Data exfiltration
- Lateral movement beyond the SMB authentication

Therefore, the impact is assessed as **successful authentication with no demonstrated post-authentication compromise** within this controlled scenario.

---

## 11. Detection Opportunities

A useful detection should correlate authentication failures rather than alerting on a single failed login.

### Detection logic

```text
Multiple Event ID 4625
        |
        +-- Same source IP
        +-- Same target account
        +-- Short time window
        |
        v
Followed by Event ID 4624
        |
        v
High-confidence authentication attack
```

A practical detection could raise severity when repeated 4625 events from the same source/account are followed by a 4624 for that account within a short time window.

### Useful Windows telemetry

- Security Event ID 4625 - failed logon
- Security Event ID 4624 - successful logon
- Logon Type
- Source Network Address
- Target account
- Logon ID
- Sysmon telemetry for subsequent process activity, if post-authentication actions occur

---

## 12. Recommended Response

For a real-world equivalent, an analyst should:

1. Validate whether the source IP is expected.
2. Determine whether the targeted account is legitimate and active.
3. Check for additional authentication failures against other accounts.
4. Investigate activity after the successful authentication.
5. Review endpoint telemetry for process, file, PowerShell, and network activity.
6. If compromise is confirmed, contain the affected account and endpoint according to the organization's incident response procedure.
7. Preserve relevant Security and Sysmon evidence.

For this lab, no containment action was required because the activity was intentionally generated by the analyst against the analyst's own VM.

---

## 13. Investigation Conclusion

The controlled investigation successfully demonstrated a network authentication attack pattern against a Windows endpoint.

Five failed SMB authentication attempts against `SOC-LabUser` were recorded as Event ID 4625 with Logon Type 3. The same source, `192.168.56.1`, subsequently authenticated successfully as `SOC-LabUser`, producing Event ID 4624 with Logon Type 3 and successful Logon ID `0x34a6b4`.

The evidence supports a controlled password-guessing scenario. However, the investigation intentionally ended after authentication, so no claim is made that the successful session resulted in command execution, persistence, privilege escalation, or data access beyond the observed SMB share enumeration.

**Final assessment:** Controlled password-guessing activity resulting in successful network authentication, with no demonstrated post-authentication compromise.

---

## 14. Lessons Learned

- A single failed login is weak evidence; correlated authentication events provide stronger detection context.
- Event ID 4625 identifies failed authentication, while Event ID 4624 confirms successful authentication.
- Logon Type 3 is important for distinguishing network authentication from interactive logons.
- Source IP is critical for attribution and correlation.
- A successful authentication following repeated failures increases the significance of the event sequence.
- Authentication success alone does not prove full endpoint compromise.
- Post-authentication telemetry is required to determine what the authenticated session actually did.

---

## 15. Evidence Handling Note

This incident was generated in a self-owned, isolated VirtualBox laboratory. The source IP, account, and target are lab artifacts and should not be represented as real-world malicious infrastructure.
