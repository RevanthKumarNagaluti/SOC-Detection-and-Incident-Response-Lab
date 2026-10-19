# SOC Detection & Incident Response Lab

A hands-on SOC home lab for practicing **security monitoring, detection engineering, incident investigation, network traffic analysis, and SIEM operations** in a controlled virtual environment.

The lab uses multiple virtual machines to simulate an attacker, monitored endpoints, network infrastructure, and a centralized SIEM.

---

## 1. Lab Objectives

This lab demonstrates:

- Centralized logging and monitoring
- Detection engineering
- Security alert investigation
- Incident triage
- Network traffic analysis
- Endpoint monitoring
- Log correlation
- MITRE ATT&CK mapping
- Incident documentation
- Detection and telemetry troubleshooting

---

## 2. Lab Architecture

The current lab consists of:

- **Kali Linux** — attacker / security testing machine
- **Windows** — monitored endpoint
- **Ubuntu Server** — web server, network sensor, and Linux telemetry source
- **Splunk Enterprise** — centralized SIEM

### Network Monitoring

- Suricata
- Wireshark

### Endpoint Monitoring

- Windows Event Logs
- Sysmon
- PowerShell logging

### Linux Monitoring

- auditd
- `/var/log/auth.log`
- `/var/log/syslog`

### Log Collection

- Splunk Universal Forwarder

---

## 3. Architecture Overview

```text
                         ATTACKER
                      ┌─────────────┐
                      │ Kali Linux  │
                      └──────┬──────┘
                             │
                       Attack Traffic
                             │
                             ▼
                   ┌──────────────────┐
                   │  Ubuntu Server   │
                   │                  │
                   │ • Suricata       │
                   │ • auditd         │
                   │ • Web Server     │
                   │ • Network Sensor │
                   └────────┬─────────┘
                            │
                            │ Telemetry
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
      ┌─────────────────┐       ┌─────────────────┐
      │ Windows Endpoint│       │  Splunk SIEM    │
      │                 │       │                 │
      │ • Sysmon        │──────▶│ • Log Analysis  │
      │ • Event Logs    │       │ • Detection     │
      │ • PowerShell    │       │ • Alerting      │
      └─────────────────┘       └─────────────────┘
```

---

## 4. Technologies Used

### SIEM

- Splunk Enterprise
- Splunk Universal Forwarder
- Splunk Processing Language (SPL)

### Windows

- Windows Event Logs
- Sysmon
- PowerShell
- PowerShell Script Block Logging
- Process creation monitoring
- Network connection monitoring

### Linux

- Ubuntu Server
- Kali Linux
- auditd
- SSH authentication logs
- Linux system logs

### Network Security

- Suricata
- Wireshark
- Nmap

### Attack Simulation

- Nmap
- Hydra
- PowerShell
- Controlled HTTP requests
- Other controlled security-testing techniques

---

# 5. Logging & Telemetry

The lab collects security telemetry from multiple sources and forwards relevant logs to Splunk.

## Windows Telemetry

Important Windows telemetry includes:

- Sysmon Event ID 1 — Process Creation
- Sysmon Event ID 3 — Network Connection
- Windows Security Events
- PowerShell Operational Logs
- PowerShell Event ID 4104 — Script Block Logging

## Linux Telemetry

The lab monitors:

```text
/var/log/auth.log
/var/log/syslog
/var/log/audit/audit.log
```

These provide visibility into authentication attempts, system activity, and Linux process/audit events.

## Network Telemetry

Suricata generates network security events through:

```text
/var/log/suricata/eve.json
```

Wireshark is used for packet-level investigation when additional network visibility is required.

---

# 6. Attack Scenarios

The current repository contains six investigation scenarios.

## Scenario 1 — Suspicious Outbound Network Activity

Investigates unusual outbound network activity from a Windows endpoint.

The scenario covers:

- Suricata alerts
- Custom Suricata rules
- Unusual destination ports
- Suspicious HTTP traffic
- HTTP User-Agent analysis
- Sysmon Event ID 3
- Splunk investigation
- Network/process correlation
- MITRE ATT&CK mapping

---

## Scenario 2 — Suspicious PowerShell Activity

Investigates potentially suspicious PowerShell activity and living-off-the-land behaviour.

The scenario covers:

- Sysmon Event ID 1
- PowerShell execution
- Command-line analysis
- `ExecutionPolicy` activity
- `IEX`
- `DownloadString`
- PowerShell Event ID 4104
- Parent/child process relationships
- Network correlation
- False-positive reduction

---

## Scenario 3 — Credential Access / Brute-Force Activity

Investigates credential-related activity and repeated failed authentication attempts.

The scenario covers:

- Windows enumeration activity
- Linux authentication logs
- SSH failed authentication
- Source IP extraction
- Splunk searches
- Brute-force detection
- Threshold-based alerting
- MITRE ATT&CK mapping

---

## Scenario 4 — Multi-Stage Attack Investigation

Investigates a multi-stage attack involving reconnaissance, web activity, credential discovery, SSH access, internal enumeration, and network pivoting.

The investigation uses multiple telemetry sources, including:

- Apache logs
- SSH authentication logs
- auditd
- Suricata
- Splunk

The scenario demonstrates how multiple events can be correlated to reconstruct an attack chain.

---

## Scenario 5 — Apache OFBiz Vulnerability Investigation

Investigates exploitation activity against a vulnerable Apache OFBiz environment within the controlled lab.

The investigation covers:

- Network scanning
- Exploitation activity
- Process execution
- Auditd telemetry
- Process relationships
- Network traffic
- Suricata
- Application logs
- Telemetry gaps
- Detection improvements

---

## Scenario 6 — React2Shell Vulnerability Investigation

Investigates exploitation activity associated with a vulnerable React/Next.js environment within the controlled lab.

The investigation uses:

- Splunk
- Audit telemetry
- Process information
- Network indicators
- Nmap activity
- Reverse-shell indicators
- Process relationships
- Timeline investigation

---

# 7. Detection Engineering

The lab uses Splunk and Suricata to detect suspicious activity.

Examples of detection areas include:

### Network Activity

- Unusual destination ports
- Suspicious HTTP requests
- Suspicious User-Agent strings
- Network reconnaissance
- Suricata alerts

### Authentication

- Repeated failed authentication
- SSH brute-force activity
- Suspicious source IP addresses

### PowerShell

- Suspicious PowerShell commands
- Encoded/obfuscated activity
- `ExecutionPolicy` bypass
- `DownloadString`
- Script Block Logging

### Endpoint Activity

- Suspicious process execution
- Parent/child process relationships
- Process-to-network correlation

---

# 8. Investigation Methodology

Investigations follow a structured SOC workflow:

```text
Alert
  ↓
Initial Triage
  ↓
Identify Source
  ↓
Identify Destination
  ↓
Review Timeline
  ↓
Analyze Relevant Logs
  ↓
Correlate Events
  ↓
Identify Attack Technique
  ↓
MITRE ATT&CK Mapping
  ↓
Assess Severity
  ↓
Recommend Response
  ↓
Document Findings
```

For each investigation, relevant evidence is collected from the available telemetry sources.

---

# 9. MITRE ATT&CK

The scenarios map observed attack activity to relevant MITRE ATT&CK techniques.

Examples include:

- Network Service Scanning
- Command and Scripting Interpreter
- PowerShell
- Brute Force
- Valid Accounts
- Remote Services
- Exploitation of Public-Facing Application
- Ingress Tool Transfer
- System Information Discovery
- Network Configuration Discovery

The specific technique mappings are documented within each scenario.

---

# 10. Repository Structure

```text
SOC-Detection-and-Incident-Response-Lab/
│
├── README.md
│
├── AttackScenario1/
│   ├── README.md
│   └── Images/
│
├── AttackScenario2/
│   ├── README.md
│   └── images/
│
├── AttackScenario3/
│   ├── README.md
│   └── images/
│
├── AttackScenario4/
│   ├── README.md
│   └── images/
│
├── AttackScenario5-CVE-2023-32113/
│   ├── README.md
│   └── images/
│
└── AttackScenario6_CVE-2025-55182_React2Shell/
    ├── README.md
    └── images/
```

Each scenario contains its own investigation documentation and supporting screenshots/evidence.

---

# 11. SOC Investigation Workflow

The overall methodology used throughout the lab is:

```text
Generate Activity
       ↓
Collect Telemetry
       ↓
Ingest into Splunk
       ↓
Detect Suspicious Activity
       ↓
Generate Alert
       ↓
Perform Initial Triage
       ↓
Investigate Evidence
       ↓
Correlate Events
       ↓
Map to MITRE ATT&CK
       ↓
Document Findings
```

The project focuses on understanding **why an alert was generated and how the available telemetry can be used to investigate it**, rather than simply generating alerts.

---

# 12. Disclaimer

All attack simulations and security testing activities in this repository are performed within a controlled and isolated laboratory environment for educational and defensive security purposes only.

Do not use the techniques demonstrated in this repository against systems or networks without explicit authorization.
