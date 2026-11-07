# Wazuh Mini SOC Lab — Windows Endpoint Monitoring & Threat Detection

## Project Overview

This project implements a virtual Security Operations Center (SOC) lab using **Wazuh** to monitor a Windows 10 endpoint, collect security events, detect suspicious activity, and monitor file integrity.

The environment was built using Oracle VirtualBox, with Ubuntu hosting the Wazuh platform and Windows 10 acting as the monitored endpoint.

The objective was to gain practical experience with endpoint monitoring, security event collection, alert analysis, and host-based threat detection.

## Objectives

- Deploy and configure a Wazuh all-in-one server.
- Connect and monitor a Windows endpoint using the Wazuh agent.
- Collect Windows Security Event logs.
- Configure and test File Integrity Monitoring (FIM).
- Detect failed Windows authentication attempts.
- Analyze generated security alerts using the Wazuh Dashboard.
- Document the lab architecture, configuration, and detection results.

## Technologies Used

| Technology | Purpose |
|---|---|
| Wazuh 4.14.10 | SIEM and XDR platform |
| Ubuntu 26.04.1 LTS | Wazuh server |
| Windows 10 | Monitored endpoint |
| Oracle VirtualBox 7.2.6 | Virtualization |
| PowerShell | Windows configuration and testing |
| Linux Terminal | Server administration |
| Windows Event Viewer | Security event verification |

## Lab Architecture

The lab consists of two virtual machines and a host-only network connection.

| Component | Configuration |
|---|---|
| Wazuh Server | Ubuntu 26.04.1 |
| Wazuh Agent | Windows 10 |
| Virtualization | VirtualBox |
| Ubuntu NAT IP | 10.0.2.15 |
| Ubuntu Internal Network IP | 192.168.100.10 |
| Ubuntu Host-only IP | 192.168.56.101 |
| Windows Host-only IP | 192.168.56.1 |
| Agent Name | Windows10-Endpoint |
| Agent ID | 001 |

### Network Design

The Ubuntu VM uses three network adapters:

- NAT: Internet connectivity for package installation and updates.
- Internal Network (CyberLab): Isolated lab network.
- Host-only: Communication between the Windows host and Ubuntu VM.

The Wazuh Dashboard is accessed through the host-only interface.

## Implemented Detection Capabilities

### 1. Windows Endpoint Monitoring

Installed and enrolled the Wazuh agent on Windows 10.

Verified that:
- The endpoint successfully registered with the Wazuh Manager.
- The Windows agent appeared as Active.
- Windows security events were forwarded to the server.

### 2. File Integrity Monitoring (FIM)

Configured Wazuh to monitor:

`C:\Wazuh-FIM-Test`

Performed controlled file operations to test detection:

| Test | Result |
|---|---|
| File creation | Detected |
| File modification | Detected |
| File deletion | Detected |

Observed Wazuh rules:

- Rule 554: File added.
- Rule 550: File checksum changed.
- Rule 553: File deleted.

These tests demonstrated Wazuh's ability to detect changes to monitored files.

### 3. Windows Failed Login Detection

Performed a controlled authentication test using a temporary Windows account named `SOC-Test`.

Windows generated Security Event ID **4625**, indicating a failed logon.

Wazuh successfully collected and analyzed the event.

**Detection details:**

| Field | Result |
|---|---|
| Windows Event ID | 4625 |
| Wazuh Rule ID | 60122 |
| Alert Description | Logon Failure - Unknown user or bad password |
| Severity Level | 5 |
| Agent | Windows10-Endpoint |
| Detection Count | 2 |

The two alerts were visible in the Wazuh Dashboard.

### 4. Windows Account and Security Event Monitoring

Collected Windows Security events and observed account-related activity, including:

- User account creation or activation.
- User account modifications.
- Security group membership enumeration.
- Successful logon events.

Account-related monitoring was explored through Wazuh's Windows security event rules.

## Wazuh Configuration

The lab included:
- Wazuh Indexer.
- Wazuh Manager.
- Wazuh Dashboard.
- Filebeat.
- Windows Wazuh Agent.

JSON event archiving was enabled on the Wazuh Manager to support raw event inspection during troubleshooting and validation.

The Manager's generated alerts were also inspected to verify detection results.

## Repository Structure

```text
wazuh-mini-soc-lab/
│
├── README.md
├── .gitignore
│
├── docs/
│   ├── architecture.md
│   ├── installation.md
│   ├── configuration.md
│   └── detection-cases.md
│
├── screenshots/
│   ├── dashboard/
│   ├── endpoint/
│   └── detections/
│
├── configs/
│   ├── agent/
│   └── manager/
│
└── reports/
    └── detection-test-results.md
```

## Skills Demonstrated

- SIEM deployment and administration.
- Linux server administration.
- Windows endpoint monitoring.
- Security event collection and analysis.
- File Integrity Monitoring.
- Authentication failure detection.
- Basic network segmentation.
- Firewall configuration.
- Log analysis and troubleshooting.
- Technical documentation.

## Security Considerations

This project was conducted in a controlled virtual lab environment.

- A dedicated test account was used for failed-login testing.
- Synthetic files were used for FIM testing.
- Private keys, certificates, passwords, and enrollment credentials must not be committed to the repository.
- Configuration examples should be sanitized before publication.

## Environment Limitations

This lab used Ubuntu 26.04.1 LTS with Wazuh 4.14.10. Ubuntu 26.04.1 was not listed among the officially supported operating systems for this Wazuh release at the time of implementation.

The environment is intended for educational and portfolio demonstration purposes, not production deployment.

## Project Outcome

Successfully deployed a functional Wazuh monitoring environment, enrolled a Windows endpoint, configured File Integrity Monitoring, and demonstrated the detection of failed authentication attempts through Windows Security Event ID 4625 and Wazuh rule 60122.

The project provided practical experience with SIEM operations, endpoint security monitoring, event analysis, and troubleshooting.

---

**Project Type:** Cybersecurity / Security Operations / SIEM  
**Environment:** Virtualized Home Lab  
**Platform:** Wazuh  
**Status:** Core detection tests completed; documentation and repository preparation in progress.
