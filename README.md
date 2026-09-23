# 🛡️ Windows Endpoint Threat Detection & Investigation Lab

A hands-on Windows endpoint security lab focused on **threat detection, endpoint telemetry analysis, SOC investigation, incident reconstruction, IOC extraction, and MITRE ATT&CK mapping**.

The project simulates suspicious endpoint activity in a controlled environment and demonstrates how a SOC analyst can move from raw Windows telemetry to a structured security investigation.

> **Project Status:** 🚧 In Progress
> This repository is being developed incrementally as each detection, investigation scenario, and analysis workflow is implemented and validated.

---

## 📌 Project Overview

Modern endpoint attacks leave traces across multiple telemetry sources rather than a single log.

This lab focuses on correlating:

* Windows Security Event Logs
* Sysmon telemetry
* PowerShell activity
* Process trees
* File and registry activity
* Authentication events
* Network connections
* Wazuh alerts
* MITRE ATT&CK techniques

The objective is to understand **why an event is suspicious**, not simply identify that an event occurred.

The investigation workflow used throughout the project is:

```text
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
```

---

# 🎯 Objectives

The primary objectives of this project are to develop practical skills in:

* Windows endpoint monitoring
* Windows Event Log analysis
* Sysmon telemetry analysis
* Process tree investigation
* PowerShell investigation
* Authentication analysis
* File and registry monitoring
* Persistence investigation
* Network activity analysis
* SIEM-based alert investigation
* Wazuh monitoring and detection
* IOC extraction
* Timeline reconstruction
* False-positive analysis
* MITRE ATT&CK mapping
* Detection engineering
* SOC incident documentation

---

# 🏗️ Lab Architecture

```text
                         ┌─────────────────────────┐
                         │     Analyst Workstation │
                         │                         │
                         │ Investigation / Analysis│
                         └────────────┬────────────┘
                                      │
                                      │
                              ┌───────▼────────┐
                              │   Wazuh Server │
                              │                │
                              │ Log Collection │
                              │ Detection      │
                              │ Alerts         │
                              │ Investigation  │
                              └───────┬────────┘
                                      │
                                Wazuh Agent
                                      │
                    ┌─────────────────▼─────────────────┐
                    │          Windows Endpoint         │
                    │                                   │
                    │ Windows Event Logs                 │
                    │ Sysmon                            │
                    │ PowerShell Logging                 │
                    │ Process Explorer                   │
                    │ Procmon                            │
                    │ File / Registry Activity           │
                    │ Network Activity                   │
                    └───────────────────────────────────┘
```

---

# 🧰 Tools & Technologies

| Technology             | Purpose                                                |
| ---------------------- | ------------------------------------------------------ |
| **Windows Event Logs** | Authentication, process, account and system activity   |
| **Sysmon**             | Detailed endpoint telemetry                            |
| **PowerShell**         | PowerShell execution and logging investigation         |
| **Process Explorer**   | Process tree and executable investigation              |
| **Procmon**            | Real-time file, registry, process and network activity |
| **Wazuh**              | SIEM, log collection, alerting and investigation       |
| **MITRE ATT&CK**       | Adversary behavior and technique mapping               |

---

# 🔎 Telemetry Sources

## Windows Security Logs

Important events investigated include:

| Event ID | Activity                                    |
| -------: | ------------------------------------------- |
|     4624 | Successful logon                            |
|     4625 | Failed logon                                |
|     4634 | Logoff                                      |
|     4648 | Explicit credential use                     |
|     4672 | Special privileges assigned                 |
|     4688 | Process creation                            |
|     4697 | Service installation                        |
|     4720 | User account creation                       |
|     4728 | User added to security-enabled global group |
|     4732 | User added to local security group          |
|     7045 | New service installed                       |
|     1102 | Security audit log cleared                  |

The goal is not to memorize Event IDs.

The goal is to understand **what investigative information each event provides and how it can be correlated with other telemetry**.

---

# 🧪 Sysmon Telemetry

Sysmon provides additional endpoint visibility that is useful for reconstructing process and attacker activity.

Key events investigated in this project include:

| Sysmon Event | Activity              |
| -----------: | --------------------- |
|            1 | Process creation      |
|            3 | Network connection    |
|            5 | Process termination   |
|            7 | Image/DLL loading     |
|            8 | CreateRemoteThread    |
|           10 | Process access        |
|           11 | File creation         |
|        12–14 | Registry activity     |
|           15 | Alternate Data Stream |
|           22 | DNS query             |
|      23 / 26 | File deletion         |

The project focuses particularly on correlating:

```text
Process Creation
      ↓
File Activity
      ↓
Registry Activity
      ↓
DNS Activity
      ↓
Network Connection
```

---

# 🧠 Investigation Methodology

Each investigation follows a consistent SOC workflow.

## 1. Alert

Identify the event or alert that initiated the investigation.

## 2. Validate

Determine whether the activity is:

* Expected
* Suspicious
* Malicious
* False positive
* Insufficient evidence

## 3. Identify

Determine:

```text
Who?
What?
When?
Where?
How?
```

## 4. Correlate

Correlate evidence across:

* Windows Event Logs
* Sysmon
* PowerShell
* Process Explorer
* Procmon
* Wazuh
* Network telemetry

## 5. Reconstruct the Process Tree

Example:

```text
explorer.exe
    └── powershell.exe
        ├── cmd.exe
        ├── whoami.exe
        └── suspicious.exe
```

The parent-child relationship provides important context that an isolated process event cannot provide.

## 6. Build a Timeline

Example:

```text
10:31:02  Successful user logon
10:31:05  PowerShell launched
10:31:07  Suspicious file created
10:31:08  Registry modification detected
10:31:10  DNS query generated
10:31:11  Outbound network connection
```

## 7. Extract IOCs

Potential indicators include:

* IP addresses
* Domains
* URLs
* File hashes
* File names
* File paths
* Registry keys
* User accounts
* Process names
* Command lines

## 8. Map to MITRE ATT&CK

Only map techniques when the available evidence supports the mapping.

## 9. Assess Impact

Determine whether the activity resulted in:

* Code execution
* Persistence
* Privilege escalation
* Credential-related activity
* Discovery
* Network communication
* Potential data access

## 10. Recommend Response

Document appropriate:

* Containment
* Eradication
* Recovery
* Monitoring
* Detection improvements

---

# 🧪 Investigation Scenarios

The lab will progressively implement controlled endpoint scenarios.

### Scenario 01 — Suspicious PowerShell Execution

Investigate:

```text
User Activity
      ↓
PowerShell
      ↓
Suspicious Command
      ↓
File Activity
      ↓
Network Activity
```

Focus areas:

* PowerShell logs
* Sysmon Event ID 1
* Parent-child relationships
* Command-line analysis
* File creation
* Network connections

---

### Scenario 02 — Authentication Attack

Investigate a sequence of failed and successful authentication events.

Example:

```text
4625
4625
4625
4625
4624
```

Questions:

* Which account was targeted?
* What was the source?
* How many failures occurred?
* Was there a successful authentication?
* What happened immediately afterward?

---

### Scenario 03 — Suspicious Process Tree

Analyze suspicious process relationships.

Example:

```text
explorer.exe
    └── powershell.exe
        └── cmd.exe
            └── suspicious.exe
```

Investigation areas:

* Parent process
* Child process
* User context
* Command line
* Executable location
* File hash
* Network activity

---

### Scenario 04 — Persistence

Investigate controlled persistence activity through mechanisms such as:

* Registry Run Keys
* Scheduled Tasks
* Windows Services
* Startup locations

Investigation objective:

```text
Initial Execution
      ↓
Persistence Mechanism
      ↓
System/User Trigger
      ↓
Payload Execution
```

---

### Scenario 05 — Suspicious Service Creation

Investigate service installation and execution.

Relevant telemetry includes:

* Windows Event Logs
* Event ID 7045
* Sysmon process events
* Process Explorer
* File activity

---

### Scenario 06 — Suspicious File Activity

Investigate a process that creates or modifies executable content.

Example:

```text
powershell.exe
      ↓
File Created
      ↓
Executable Execution
      ↓
Network Connection
```

---

### Scenario 07 — Defense Evasion

Investigate controlled examples involving behaviors such as:

* Log clearing
* File deletion
* Suspicious PowerShell execution
* Process masquerading concepts

The objective is to understand **what evidence is generated, what evidence may disappear, and how an analyst can correlate surviving telemetry**.

---

# 🧬 MITRE ATT&CK Mapping

Observed behaviors will be mapped to relevant MITRE ATT&CK techniques.

Examples include:

| Behavior                           | Technique |
| ---------------------------------- | --------- |
| Network scanning                   | T1046     |
| PowerShell                         | T1059.001 |
| Windows Command Shell              | T1059.003 |
| Brute Force                        | T1110     |
| Scheduled Task                     | T1053.005 |
| Windows Service                    | T1543.003 |
| Registry Run Keys / Startup Folder | T1547.001 |
| Masquerading                       | T1036     |

> ATT&CK mappings will be based on observed evidence rather than automatically assigning a technique to every event.

---

# 🚨 Detection & Investigation Philosophy

A central principle of this lab is:

> **A suspicious event is not automatically a confirmed compromise.**

For example:

```text
powershell.exe
```

does not independently establish malicious activity.

The investigation should consider:

```text
Parent Process
      +
Command Line
      +
User
      +
File Activity
      +
Registry Activity
      +
Network Activity
      +
Timing
      +
Context
```

The project therefore emphasizes **behavioral correlation rather than single-event detection**.

---

# 🧹 False Positive Analysis

Every significant detection should be evaluated for false positives.

Example:

```text
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
```

This helps distinguish:

```text
True Positive
False Positive
Benign / Expected Activity
Insufficient Evidence
```

---

# 📊 IOC Extraction

Each investigation should produce a structured IOC record.

Example:

| IOC Type | Value        | Source             | Confidence |
| -------- | ------------ | ------------------ | ---------- |
| IP       | `<IP>`       | Sysmon Event 3     | Medium     |
| Domain   | `<domain>`   | Sysmon Event 22    | Medium     |
| Hash     | `<SHA256>`   | File analysis      | High       |
| File     | `<filename>` | Sysmon Event 11    | High       |
| Registry | `<key>`      | Sysmon Event 12–14 | High       |
| Process  | `<process>`  | Sysmon Event 1     | Medium     |

Actual values will be documented in the individual investigation reports.

---

# 📁 Repository Structure

The repository will evolve as the lab is implemented.

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
```

Directories and files will be added as the corresponding lab components are implemented.

---

# 📚 Skills Demonstrated

This project is designed to demonstrate practical capability in:

* Windows Security Event Log analysis
* Sysmon investigation
* Endpoint telemetry analysis
* Process tree analysis
* PowerShell investigation
* Authentication monitoring
* File and registry investigation
* Persistence analysis
* Network activity analysis
* SIEM investigation
* Wazuh
* IOC extraction
* Timeline reconstruction
* False-positive analysis
* MITRE ATT&CK
* Detection engineering
* Incident response documentation

---

# 🔐 Security & Ethical Use

This project is intended for **defensive cybersecurity education and authorized laboratory use only**.

All activity should be performed inside an isolated and controlled environment.

No unauthorized systems, accounts, networks, or data should be targeted.

Where attack-like behavior is simulated, it should be performed only against systems owned or explicitly authorized for testing.

---

# ⚠️ Project Limitations

This is an educational home-lab environment and should not be interpreted as a production-grade EDR or SOC platform.

Detection accuracy depends on:

* Logging configuration
* Sysmon configuration
* Wazuh rules
* Endpoint configuration
* Available telemetry
* Investigation context

A single alert should not automatically be treated as proof of compromise.

---

# 🚀 Project Roadmap

```text
[ ] Windows endpoint preparation
[ ] Windows Event Log fundamentals
[ ] Sysmon deployment
[ ] Sysmon telemetry validation
[ ] Process Explorer investigation
[ ] Procmon investigation
[ ] PowerShell logging
[ ] File activity investigation
[ ] Registry investigation
[ ] Persistence investigation
[ ] Authentication investigation
[ ] Network telemetry investigation
[ ] Wazuh agent integration
[ ] Wazuh detection rules
[ ] Controlled attack scenarios
[ ] Alert triage
[ ] Timeline reconstruction
[ ] IOC extraction
[ ] False-positive analysis
[ ] MITRE ATT&CK mapping
[ ] Detection tuning
[ ] Incident reports
[ ] Final investigation summary
```

---

# 🎓 Learning Outcome

By completing this project, the objective is to move beyond simply knowing individual security tools and develop the ability to investigate endpoint activity as a SOC analyst.

The final workflow should be:

```text
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
```

---

## 👤 Author

**Suhas Jadhav**

Cybersecurity / SOC Analyst Portfolio Project

GitHub: [SuhasJadhavSJ](https://github.com/SuhasJadhavSJ)

---

## 📌 Project Focus

**Windows Endpoint Security · Threat Detection · Threat Investigation · SOC Analysis · Sysmon · Wazuh · PowerShell · MITRE ATT&CK**
