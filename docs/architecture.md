# Lab Architecture — Wazuh Mini SOC

## 1. Overview

The Wazuh Mini SOC Lab is a virtualized security monitoring environment built using Oracle VirtualBox.

It consists of:
- An Ubuntu virtual machine hosting the Wazuh all-in-one deployment.
- A Windows 10 endpoint monitored by the Wazuh agent.
- VirtualBox network adapters providing NAT, isolated internal communication, and host-only connectivity.

The purpose of this architecture is to collect endpoint security events centrally and analyze them through the Wazuh Dashboard.

## 2. Infrastructure Components

| Component | Configuration | Role |
|---|---|---|
| Ubuntu VM | Ubuntu 26.04.1 LTS | Wazuh server |
| Wazuh Indexer | Version 4.14.10 | Security event indexing |
| Wazuh Manager | Version 4.14.10 | Event analysis and alert generation |
| Wazuh Dashboard | Version 4.14.10 | Monitoring and visualization |
| Filebeat | Version 7.10.2 | Event forwarding |
| Windows VM/endpoint | Windows 10 | Monitored endpoint |
| Wazuh Agent | Version 4.14.10 | Endpoint event collection |
| VirtualBox | Version 7.2.6 | Virtualization platform |

## 3. Network Topology

### Ubuntu Server Network Interfaces

| Adapter | Network Mode | IP Address | Purpose |
|---|---|---|---|
| Adapter 1 | NAT | 10.0.2.15 | Internet access |
| Adapter 2 | Internal Network (`CyberLab`) | 192.168.100.10 | Isolated lab communication |
| Adapter 3 | Host-only | 192.168.56.101 | Host-to-server connectivity |

### Windows Host Connectivity

| Component | IP Address |
|---|---|
| Windows host-only adapter | 192.168.56.1 |
| Ubuntu host-only adapter | 192.168.56.101 |

The Wazuh Dashboard is accessed from Windows through the Ubuntu host-only address:

`https://192.168.56.101`

The lab uses a self-signed certificate, so the browser may display a certificate warning.

## 4. Architecture Diagram

```text
                  Windows 10 Host
                  192.168.56.1
                         |
                         | Host-only Network
                         |
                         v
              +------------------------+
              |     Ubuntu VM          |
              |     192.168.56.101     |
              |                        |
              |   Wazuh All-in-One     |
              |                        |
              |  +------------------+  |
              |  | Wazuh Manager    |  |
              |  +------------------+  |
              |  | Wazuh Indexer    |  |
              |  +------------------+  |
              |  | Wazuh Dashboard  |  |
              |  +------------------+  |
              |  | Filebeat         |  |
              |  +------------------+  |
              +------------------------+
                         ^
                         |
                    Agent Events
                         |
                         |
              +------------------------+
              | Windows 10 Endpoint    |
              | Wazuh Agent            |
              | Windows10-Endpoint     |
              | Agent ID: 001          |
              +------------------------+
```

## 5. Communication and Ports

| Port | Protocol | Purpose |
|---|---|---|
| 443 | TCP | Wazuh Dashboard HTTPS |
| 1514 | TCP | Wazuh agent event communication |
| 1515 | TCP | Wazuh agent enrollment |
| 9200 | TCP | Wazuh Indexer API |

The lab firewall was configured to restrict inbound access to the required services.

## 6. Event Collection and Detection Flow

1. The Windows Wazuh Agent collects configured Windows Security events and file integrity events.
2. Events are transmitted to the Wazuh Manager.
3. The Manager decodes and evaluates incoming events against its rules.
4. Matching events generate security alerts.
5. Alerts are indexed and made available through the Wazuh Dashboard.
6. The analyst investigates the alerts through the dashboard or manager log files.

## 7. Network Segmentation

Three network interfaces were configured on the Ubuntu VM:

- NAT provides internet access for installation and updates.
- Internal Network provides an isolated VirtualBox network.
- Host-only provides communication between the Windows host and Ubuntu server.

This separation allows the lab services to be accessed without exposing the dashboard directly to a public network.

## 8. Architecture Limitations

This is an educational, single-server deployment. The Wazuh components run together on one Ubuntu VM rather than on separate production servers.

Ubuntu 26.04.1 LTS was used with Wazuh 4.14.10, although this OS release was not listed among the supported operating systems for that Wazuh release at the time of implementation.

This configuration is intended for controlled lab use and should not be treated as a production reference architecture.

## 9. Summary

The architecture provides a centralized security monitoring environment in which a Windows endpoint forwards events to a Wazuh server for analysis, indexing, and visualization.

The design demonstrates practical understanding of virtualization, network segmentation, endpoint monitoring, centralized logging, and SIEM architecture.