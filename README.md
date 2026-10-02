**# 🛡️ Windows Endpoint Threat Detection & Investigation Lab**

A hands-on Windows endpoint security lab focused on **\*\*threat detection, endpoint telemetry analysis, SOC investigation, incident reconstruction, IOC extraction, and MITRE ATT&CK mapping\*\***.

The project simulates suspicious endpoint activity in a controlled environment and demonstrates how a SOC analyst can move from raw Windows telemetry to a structured security investigation.

\> **\*\*Project Status:\*\*** 🚧 In Progress

\> The Windows 10 VirtualBox lab environment has been prepared and documented. The project has now moved from environment setup into hands-on endpoint investigation, telemetry analysis, and controlled investigation scenarios.

\---

**## 📌 Project Overview**

Modern endpoint attacks leave traces across multiple telemetry sources rather than a single log.

This lab focuses on correlating:

\* Windows Security Event Logs

\* Sysmon telemetry

\* PowerShell activity

\* Process trees

\* File and registry activity

\* Authentication events

\* Network connections

\* Wazuh alerts

\* MITRE ATT&CK techniques

The objective is to understand **\*\*why an event is suspicious\*\***, not simply identify that an event occurred.

The investigation workflow used throughout the project is:

\`\`\`text

Endpoint Activity

       ↓

Telemetry Collection

       ↓

Detection / Alert

       ↓

Alert Validation

       ↓

Process & Event Correlation

       ↓

Timeline Reconstruction

       ↓

IOC Extraction

       ↓

MITRE ATT&CK Mapping

       ↓

Impact Assessment

       ↓

Containment Recommendations

       ↓

Detection Improvement

\`\`\`

\---

**# 🎯 Objectives**

The primary objectives of this project are to develop practical skills in:

\* Windows endpoint monitoring

\* Windows Event Log analysis

\* Sysmon telemetry analysis

\* Process tree investigation

\* PowerShell investigation

\* Authentication analysis

\* File and registry monitoring

\* Persistence investigation

\* Network activity analysis

\* SIEM-based alert investigation

\* Wazuh monitoring and detection

\* IOC extraction

\* Timeline reconstruction

\* False-positive analysis

\* MITRE ATT&CK mapping

\* Detection engineering

\* SOC incident documentation

\---

**# 🏗️ Lab Architecture

The project uses a controlled Windows 10 virtual machine as the endpoint investigation environment. The current phase focuses on collecting and understanding native Windows telemetry and Sysmon data before integrating the endpoint with Wazuh.

```text
                         ┌──────────────────────────────┐
                         │      Host Machine            │
                         │       VirtualBox             │
                         └──────────────┬───────────────┘
                                        │
                                        ▼
                         ┌──────────────────────────────┐
                         │       Windows 10 VM          │
                         │                              │
                         │  Windows Event Logs          │
                         │  Sysmon                      │
                         │  PowerShell                  │
                         │  Process Explorer            │
                         │  Procmon                     │
                         └──────────────┬───────────────┘
                                        │
                                        ▼
                         ┌──────────────────────────────┐
                         │ Endpoint Investigation       │
                         │                              │
                         │ Event Analysis               │
                         │ Event Correlation            │
                         │ Timeline Reconstruction      │
                         │ IOC Extraction               │
                         │ MITRE ATT&CK Mapping         │
                         └──────────────┬───────────────┘
                                        │
                                        ▼
                         ┌──────────────────────────────┐
                         │       Wazuh Integration      │
                         │          (Later Phase)       │
                         └──────────────────────────────┘
```

### Current Environment

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Endpoint OS | Windows 10 64-bit |
| VM Memory | 3072 MB |
| VM Processors | 2 |
| Virtual Disk | 50 GB |
| Network | NAT |
| Investigation Tools | Event Viewer, PowerShell, Sysmon, Process Explorer, Procmon |
| SIEM | Wazuh, planned integration phase |

The environment configuration has been captured and documented in the repository under `screenshots/00-environment/`.

---

# 🧰 Tools & Technologies**

\| Technology             | Purpose                                                |

\| ---------------------- | ------------------------------------------------------ |

\| **\*\*Windows Event Logs\*\*** | Authentication, process, account and system activity   |

\| **\*\*Sysmon\*\***             | Detailed endpoint telemetry                            |

\| **\*\*PowerShell\*\***         | PowerShell execution and logging investigation         |

\| **\*\*Process Explorer\*\***   | Process tree and executable investigation              |

\| **\*\*Procmon\*\***            | Real-time file, registry, process and network activity |

\| **\*\*Wazuh\*\***              | SIEM, log collection, alerting and investigation       |

\| **\*\*MITRE ATT&CK\*\***       | Adversary behavior and technique mapping               |

\---

**# 🖥️ Lab Environment Setup & Evidence

The initial Windows endpoint environment has been configured in VirtualBox before beginning the investigation phase. These screenshots document the virtual machine configuration and Windows operating system details used for the lab.

## VirtualBox Environment

### VirtualBox Overview

![VirtualBox VM Overview](screenshots/00-environment/01-virtualbox-overview.png)

Shows the Windows 10 virtual machine, operating system type, allocated memory, processor count, virtual disk, graphics configuration, and NAT networking.

### VirtualBox System Configuration

![VirtualBox System Configuration](screenshots/00-environment/02-virtualbox-system.png)

Documents the VM's motherboard and processor configuration.

### VirtualBox Network Configuration

![VirtualBox Network Configuration](screenshots/00-environment/03-virtualbox-network.png)

Documents the virtual network adapter configuration used by the Windows endpoint.

## Windows Endpoint Evidence

### Windows System Information

![Windows System Information](screenshots/00-environment/04-windows-msinfo32.png)

Documents the Windows endpoint's operating system and hardware information from `msinfo32`.

### Windows Version

![Windows Version](screenshots/00-environment/05-windows-version.png)

Documents the Windows version/build running inside the virtual machine.

## Why This Evidence Is Included

The environment screenshots establish the controlled endpoint used for the investigations. They provide reproducibility and context for the telemetry collected later in the project.

The evidence chain will now progress from:

```text
Lab Environment
      ↓
Windows Logging
      ↓
Sysmon Deployment
      ↓
Telemetry Validation
      ↓
Investigation 001
      ↓
Investigation 002
      ↓
Investigation 003
      ↓
Additional Investigation Scenarios
      ↓
Wazuh Detection Engineering
```

# 🔎 Telemetry Sources**

**## Windows Security Logs**

Important events investigated include:

\| Event ID | Activity                                    |

\| -------: | ------------------------------------------- |

\|     4624 | Successful logon                            |

\|     4625 | Failed logon                                |

\|     4634 | Logoff                                      |

\|     4648 | Explicit credential use                     |

\|     4672 | Special privileges assigned                 |

\|     4688 | Process creation                            |

\|     4697 | Service installation                        |

\|     4720 | User account creation                       |

\|     4728 | User added to security-enabled global group |

\|     4732 | User added to local security group          |

\|     7045 | New service installed                       |

\|     1102 | Security audit log cleared                  |

The goal is not to memorize Event IDs.

The goal is to understand **\*\*what investigative information each event provides and how it can be correlated with other telemetry\*\***.

\---

**# 🧪 Sysmon Telemetry**

Sysmon provides additional endpoint visibility that is useful for reconstructing process and attacker activity.

Key events investigated in this project include:

\| Sysmon Event | Activity              |

\| -----------: | --------------------- |

\|            1 | Process creation      |

\|            3 | Network connection    |

\|            5 | Process termination   |

\|            7 | Image/DLL loading     |

\|            8 | CreateRemoteThread    |

\|           10 | Process access        |

\|           11 | File creation         |

\|        12–14 | Registry activity     |

\|           15 | Alternate Data Stream |

\|           22 | DNS query             |

\|      23 / 26 | File deletion         |

The project focuses particularly on correlating:

\`\`\`text

Process Creation

      ↓

File Activity

      ↓

Registry Activity

      ↓

DNS Activity

      ↓

Network Connection

\`\`\`

\---

**# 🧠 Investigation Methodology**

Each investigation follows a consistent SOC workflow.

**## 1. Alert**

Identify the event or alert that initiated the investigation.

**## 2. Validate**

Determine whether the activity is:

\* Expected

\* Suspicious

\* Malicious

\* False positive

\* Insufficient evidence

**## 3. Identify**

Determine:

\`\`\`text

Who?

What?

When?

Where?

How?

\`\`\`

**## 4. Correlate**

Correlate evidence across:

\* Windows Event Logs

\* Sysmon

\* PowerShell

\* Process Explorer

\* Procmon

\* Wazuh

\* Network telemetry

**## 5. Reconstruct the Process Tree**

Example:

\`\`\`text

explorer.exe

    └── powershell.exe

        ├── cmd.exe

        ├── whoami.exe

        └── suspicious.exe

\`\`\`

The parent-child relationship provides important context that an isolated process event cannot provide.

**## 6. Build a Timeline**

Example:

\`\`\`text

10:31:02  Successful user logon

10:31:05  PowerShell launched

10:31:07  Suspicious file created

10:31:08  Registry modification detected

10:31:10  DNS query generated

10:31:11  Outbound network connection

\`\`\`

**## 7. Extract IOCs**

Potential indicators include:

\* IP addresses

\* Domains

\* URLs

\* File hashes

\* File names

\* File paths

\* Registry keys

\* User accounts

\* Process names

\* Command lines

**## 8. Map to MITRE ATT&CK**

Only map techniques when the available evidence supports the mapping.

**## 9. Assess Impact**

Determine whether the activity resulted in:

\* Code execution

\* Persistence

\* Privilege escalation

\* Credential-related activity

\* Discovery

\* Network communication

\* Potential data access

**## 10. Recommend Response**

Document appropriate:

\* Containment

\* Eradication

\* Recovery

\* Monitoring

\* Detection improvements

\---

**# 🧪 Investigation Scenarios

The investigation phase is being implemented as controlled, evidence-driven endpoint cases. Each case follows the same learning and investigation cycle:

```text
LEARN
  ↓
SIMULATE
  ↓
OBSERVE
  ↓
INVESTIGATE
  ↓
CORRELATE
  ↓
DOCUMENT
```

The objective is to understand the telemetry and reasoning behind each investigation, not simply generate screenshots.

## Investigation 001 — Authentication Activity

Investigate a controlled sequence of failed and successful Windows authentication events.

Primary telemetry:

- Security Event ID 4625, failed logon
- Security Event ID 4624, successful logon
- Logon Type
- Account information
- Logon ID
- Source network information
- Timestamp correlation

Investigation questions:

- Which account was targeted?
- What type of logon occurred?
- Where did the authentication attempt originate?
- How many failures occurred?
- Was there a successful authentication?
- What happened immediately afterward?
- Does the evidence support suspicious activity?

## Investigation 002 — Suspicious PowerShell Execution

Investigate controlled PowerShell execution and correlate process telemetry.

Primary telemetry:

- PowerShell activity
- Sysmon Event ID 1
- Parent process
- Child processes
- Command line
- User context
- File activity
- Network activity where present

Investigation questions:

- What launched PowerShell?
- What command was executed?
- Which user executed it?
- What child processes were created?
- Were files created or modified?
- Did any related network activity occur?
- Does the complete process chain provide suspicious context?

## Investigation 003 — Suspicious File Execution

Investigate controlled file creation and execution.

Primary telemetry:

- Sysmon Event ID 11, file creation
- Sysmon Event ID 1, process creation
- Process tree
- File path
- User context
- Command line
- Related network activity

Investigation questions:

- Which process created the file?
- Where was the file created?
- Was the file subsequently executed?
- Which process executed it?
- What parent-child relationship was observed?
- Are there useful IOCs?
- What MITRE ATT&CK techniques are supported by the evidence?

## Future Investigation Scenarios

Additional scenarios will be implemented after the first three investigations:

### Scenario 04 — Persistence

Controlled persistence investigation involving mechanisms such as:

- Registry Run Keys
- Scheduled Tasks
- Windows Services
- Startup locations

### Scenario 05 — Suspicious Service Creation

Investigation of service installation and execution using:

- Windows Event ID 7045
- Sysmon process telemetry
- Process Explorer
- File activity

### Scenario 06 — Suspicious File Activity

Investigation of a process that creates or modifies executable content.

### Scenario 07 — Defense Evasion

Controlled examples involving behaviors such as:

- Security log clearing
- File deletion
- Suspicious PowerShell activity
- Process masquerading concepts

All scenarios will be documented as controlled laboratory exercises and will be distinguished from observations originating from external datasets.

---

# 🧬 MITRE ATT&CK Mapping**

Observed behaviors will be mapped to relevant MITRE ATT&CK techniques.

Examples include:

\| Behavior                           | Technique |

\| ---------------------------------- | --------- |

\| Network scanning                   | T1046     |

\| PowerShell                         | T1059.001 |

\| Windows Command Shell              | T1059.003 |

\| Brute Force                        | T1110     |

\| Scheduled Task                     | T1053.005 |

\| Windows Service                    | T1543.003 |

\| Registry Run Keys / Startup Folder | T1547.001 |

\| Masquerading                       | T1036     |

\> ATT&CK mappings will be based on observed evidence rather than automatically assigning a technique to every event.

\---

**# 🚨 Detection & Investigation Philosophy**

A central principle of this lab is:

\> **\*\*A suspicious event is not automatically a confirmed compromise.\*\***

For example:

\`\`\`text

powershell.exe

\`\`\`

does not independently establish malicious activity.

The investigation should consider:

\`\`\`text

Parent Process

      \+

Command Line

      \+

User

      \+

File Activity

      \+

Registry Activity

      \+

Network Activity

      \+

Timing

      \+

Context

\`\`\`

The project therefore emphasizes **\*\*behavioral correlation rather than single-event detection\*\***.

\---

**# 🧹 False Positive Analysis**

Every significant detection should be evaluated for false positives.

Example:

\`\`\`text

Detection

   ↓

Evidence

   ↓

Expected administrative activity?

   ↓

Known software behavior?

   ↓

Suspicious context?

   ↓

Related activity?

   ↓

Analyst conclusion

\`\`\`

This helps distinguish:

\`\`\`text

True Positive

False Positive

Benign / Expected Activity

Insufficient Evidence

\`\`\`

\---

**# 📊 IOC Extraction**

Each investigation should produce a structured IOC record.

Example:

\| IOC Type | Value        | Source             | Confidence |

\| -------- | ------------ | ------------------ | ---------- |

\| IP       | \`\<IP>\`       | Sysmon Event 3     | Medium     |

\| Domain   | \`\<domain>\`   | Sysmon Event 22    | Medium     |

\| Hash     | \`\<SHA256>\`   | File analysis      | High       |

\| File     | \`\<filename>\` | Sysmon Event 11    | High       |

\| Registry | \`\<key>\`      | Sysmon Event 12–14 | High       |

\| Process  | \`\<process>\`  | Sysmon Event 1     | Medium     |

Actual values will be documented in the individual investigation reports.

\---

**# 📁 Repository Structure

The repository is organized to separate lab setup, evidence, investigations, detection engineering, and supporting documentation.

```text
Windows-Endpoint-Threat-Detection-Investigation-Lab/
│
├── README.md
│
├── architecture/
│   └── architecture.md
│
├── setup/
│   ├── windows/
│   ├── sysmon/
│   ├── powershell/
│   └── wazuh/
│
├── scenarios/
│   ├── 01-powershell/
│   ├── 02-authentication/
│   ├── 03-process-execution/
│   ├── 04-persistence/
│   ├── 05-service-creation/
│   ├── 06-file-activity/
│   └── 07-defense-evasion/
│
├── investigations/
│   ├── incident-001/
│   ├── incident-002/
│   └── incident-003/
│
├── detections/
│   └── wazuh-rules/
│
├── iocs/
│   └── extracted-iocs.md
│
├── mitre/
│   └── attack-mapping.md
│
├── reports/
│   ├── incident-reports/
│   └── executive-summary.md
│
└── screenshots/
    ├── 00-environment/
    │   ├── 01-virtualbox-overview.png
    │   ├── 02-virtualbox-system.png
    │   ├── 03-virtualbox-network.png
    │   ├── 04-windows-msinfo32.png
    │   └── 05-windows-version.png
    │
    ├── 01-lab-setup/
    ├── 001-authentication/
    ├── 002-powershell/
    └── 003-file-execution/
```

The `00-environment` screenshots document the completed Windows VM setup. Investigation screenshots will be added only when the corresponding evidence is generated and analyzed.

---

# 📚 Skills Demonstrated**

This project is designed to demonstrate practical capability in:

\* Windows Security Event Log analysis

\* Sysmon investigation

\* Endpoint telemetry analysis

\* Process tree analysis

\* PowerShell investigation

\* Authentication monitoring

\* File and registry investigation

\* Persistence analysis

\* Network activity analysis

\* SIEM investigation

\* Wazuh

\* IOC extraction

\* Timeline reconstruction

\* False-positive analysis

\* MITRE ATT&CK

\* Detection engineering

\* Incident response documentation

\---

**# 🔐 Security & Ethical Use**

This project is intended for **\*\*defensive cybersecurity education and authorized laboratory use only\*\***.

All activity should be performed inside an isolated and controlled environment.

No unauthorized systems, accounts, networks, or data should be targeted.

Where attack-like behavior is simulated, it should be performed only against systems owned or explicitly authorized for testing.

\---

**# ⚠️ Project Limitations**

This is an educational home-lab environment and should not be interpreted as a production-grade EDR or SOC platform.

Detection accuracy depends on:

\* Logging configuration

\* Sysmon configuration

\* Wazuh rules

\* Endpoint configuration

\* Available telemetry

\* Investigation context

A single alert should not automatically be treated as proof of compromise.

\---

**# 🚀 Project Roadmap

```text
[✓] Windows endpoint preparation
[✓] VirtualBox environment configuration
[✓] Windows system/version documentation
[ ] Windows Event Log fundamentals
[ ] Sysmon deployment
[ ] Sysmon telemetry validation
[ ] Investigation 001 — Authentication
[ ] Investigation 002 — PowerShell
[ ] Investigation 003 — File Execution
[ ] Process Explorer investigation
[ ] Procmon investigation
[ ] File activity investigation
[ ] Registry investigation
[ ] Persistence investigation
[ ] Network telemetry investigation
[ ] Wazuh agent integration
[ ] Wazuh detection rules
[ ] Additional controlled investigation scenarios
[ ] Alert triage
[ ] Timeline reconstruction
[ ] IOC extraction
[ ] False-positive analysis
[ ] MITRE ATT&CK mapping
[ ] Detection tuning
[ ] Incident reports
[ ] Final investigation summary
```

## Current Project Phase

**Phase: Endpoint Investigation**

The lab environment has been configured and documented. The project is now moving into telemetry deployment, validation, and investigation.

### Immediate sequence

```text
1. Deploy Sysmon
2. Validate Sysmon telemetry
3. Learn Sysmon Event ID 1
4. Complete Investigation 001
5. Complete Investigation 002
6. Complete Investigation 003
7. Expand into persistence/network/service investigations
8. Integrate Wazuh
9. Build detection rules
10. Produce final incident documentation
```

# 🎓 Learning Outcome**

By completing this project, the objective is to move beyond simply knowing individual security tools and develop the ability to investigate endpoint activity as a SOC analyst.

The final workflow should be:

\`\`\`text

             ┌────────────────────┐

             │ Windows Endpoint   │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Telemetry          │

             │ Event Logs / Sysmon│

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Detection          │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Triage             │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Investigation      │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Timeline            │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ IOC Extraction     │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ MITRE ATT&CK       │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Impact Assessment  │

             └─────────┬──────────┘

                       ↓

             ┌────────────────────┐

             │ Response / Tuning  │

             └────────────────────┘

\`\`\`

\---

**## 📍 Current Project Status

```text
PROJECT STATUS: 🟡 INVESTIGATION PHASE

Environment:
Windows 10 / VirtualBox

Environment Setup:
✓ VirtualBox configuration documented
✓ Windows system information documented
✓ Windows version documented

Current Focus:
Sysmon deployment → telemetry validation → endpoint investigation

Today's Investigation Target:
001 Authentication
002 PowerShell
003 Suspicious File Execution
```

The project has now moved beyond environment preparation. The next work will focus on understanding and investigating endpoint telemetry while documenting the reasoning behind each finding.

---

# 👤 Author**

**\*\*Suhas Jadhav\*\***

Cybersecurity / SOC Analyst Portfolio Project

GitHub: [SuhasJadhavSJ]\(https\://github.com/SuhasJadhavSJ)

\---

**## 📌 Project Focus**

**\*\*Windows Endpoint Security · Threat Detection · Threat Investigation · SOC Analysis · Sysmon · Wazuh · PowerShell · MITRE ATT&CK\*\***