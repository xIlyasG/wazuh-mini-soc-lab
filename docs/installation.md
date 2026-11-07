# Installation Guide

## 1. Lab Overview

This guide documents the installation and initial deployment of the Wazuh Mini SOC Lab.

The lab uses:

- **Wazuh version:** 4.14.10
- **Filebeat version:** 7.10.2
- **Server operating system:** Ubuntu 26.04.1 LTS
- **Monitored endpoint:** Windows 10 host
- **Virtualization:** Oracle VirtualBox 7.2.6

The Wazuh server components are installed on the Ubuntu virtual machine using an all-in-one deployment. The Windows 10 host runs the Wazuh agent.

## 2. Prerequisites

Before installation, the following resources were available:

- A Windows 10 host computer
- Oracle VirtualBox
- An Ubuntu virtual machine with 8 GB RAM, 4 CPUs, and a 60 GB virtual disk
- Internet connectivity for downloading packages
- Network connectivity between the Ubuntu server and Windows host

## 3. Ubuntu Virtual Machine Preparation

The Ubuntu virtual machine was configured with three network interfaces:

| Interface purpose | Network mode | IPv4 address |
|---|---|---|
| Internet access | NAT | 10.0.2.15 |
| Isolated lab network | Internal Network (`CyberLab`) | 192.168.100.10 |
| Windows host connectivity | Host-only Adapter | 192.168.56.101 |

The Windows host-only interface used `192.168.56.1`.

Connectivity between the Windows host and Ubuntu server was verified before proceeding.

## 4. Wazuh Server Installation

Wazuh was deployed as an all-in-one installation on Ubuntu.

The installed server components were:

- Wazuh Indexer 4.14.10
- Wazuh Manager 4.14.10
- Wazuh Dashboard 4.14.10

Filebeat 7.10.2 was also installed and configured to forward Wazuh alerts to the indexer.

**Operating-system compatibility note:** Ubuntu 26.04.1 LTS was not listed among the officially supported operating systems for Wazuh 4.14.10 at the time this lab was implemented. The installation was completed for educational and testing purposes. This environment should not be treated as a production deployment.

## 5. Service Verification

After installation, the services were checked on Ubuntu.

The following services were confirmed active:

- `wazuh-indexer`
- `wazuh-manager`
- `wazuh-dashboard`
- `filebeat`

The service status was verified using the Linux `systemctl` utility.

## 6. Dashboard Access

The Wazuh Dashboard was accessed from the Windows host using:

`https://192.168.56.101`

The dashboard uses HTTPS. A browser security warning may appear because the lab uses a self-signed certificate.

Successful dashboard access confirmed connectivity between the Windows host and the Ubuntu server.

## 7. Windows Agent Installation

The matching Wazuh Windows agent package was installed:

`wazuh-agent-4.14.10-1.msi`

The agent configuration file is located at:

`C:\Program Files (x86)\ossec-agent\ossec.conf`

The manager address was configured as:

`192.168.56.101`

The endpoint was registered with the following identity:

- **Agent name:** `Windows10-Endpoint`
- **Agent ID:** `001`

The agent enrollment service used TCP port `1515`.

After enrollment, the Windows Wazuh service (`WazuhSvc`) was confirmed running.

## 8. Agent Connectivity Verification

The endpoint's connection was verified from the Wazuh Manager.

The agent appeared as active in the manager's agent list and in the Wazuh Dashboard.

The Windows host communicated with the manager through the host-only network.

## 9. Network and Firewall Configuration

The Ubuntu firewall was configured to restrict incoming connections.

The lab allowed the required communication from the host-only network:

| Port | Protocol | Purpose |
|---|---|---|
| 443 | TCP | Wazuh Dashboard access |
| 1514 | TCP | Wazuh agent event communication |
| 1515 | TCP | Wazuh agent enrollment |

Port `9200` is used for internal Wazuh Indexer communication and was not intended to be exposed to the Windows host.

Connectivity to the required ports was tested from Windows.

## 10. Installation Outcome

The installation phase was considered successful after verifying:

- Wazuh server components were active.
- Filebeat was active.
- The dashboard was accessible from Windows.
- The Windows agent was enrolled successfully.
- The endpoint appeared as active in the dashboard.
- Required network connectivity was available.

The lab was then ready for endpoint monitoring, File Integrity Monitoring, and security-event detection tests.