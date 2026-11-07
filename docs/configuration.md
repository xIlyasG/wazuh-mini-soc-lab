# Configuration Guide

## 1. Overview

This document describes the main configurations applied in the Wazuh Mini SOC Lab.

The configuration covers:

- Windows endpoint enrollment and communication
- File Integrity Monitoring (FIM)
- Windows Security event collection
- Wazuh Manager JSON event archiving
- Firewall access restrictions

## 2. Windows Agent Configuration

The Wazuh agent was installed on the Windows 10 host.

Configuration file:

`C:\Program Files (x86)\ossec-agent\ossec.conf`

The manager connection was configured to use the Ubuntu server's host-only IP address:

`192.168.56.101`

The endpoint was registered with:

- Agent name: `Windows10-Endpoint`
- Agent ID: `001`

The agent service was verified as running through the Windows Services system.

## 3. File Integrity Monitoring

File Integrity Monitoring was configured to monitor a dedicated test directory on Windows:

`C:\Wazuh-FIM-Test`

The following configuration was used inside the agent's `syscheck` section:

```xml
<directories realtime="yes">C:\Wazuh-FIM-Test</directories>
```

The `realtime="yes"` option enables real-time monitoring of the configured directory.

### FIM validation

Three file operations were tested:

| Test | Expected detection | Observed Wazuh rule |
|---|---|---|
| Create a file | File added | 554 |
| Modify a file | File checksum changed | 550 |
| Delete a file | File deleted | 553 |

All three operations generated corresponding Wazuh alerts.

## 4. Windows Security Event Collection

Windows Security events were collected through the Wazuh agent's Windows Event Channel integration.

The Security event log was used to monitor authentication-related activity.

A failed authentication attempt was generated using a dedicated test account. The attempt was rejected by Windows and generated Security Event ID `4625`.

The event was subsequently received and analyzed by the Wazuh Manager.

## 5. Wazuh Manager JSON Event Archiving

JSON event archiving was enabled in the Wazuh Manager configuration.

The relevant setting was:

```xml
<logall_json>yes</logall_json>
```

The setting was validated using the Wazuh analysis configuration test before the Manager was restarted.

The archive file was verified at:

`/var/ossec/logs/archives/archives.json`

The archive contained Windows endpoint events decoded by `windows_eventchannel`.

**Security note:** Raw archives can contain usernames, host information, IP addresses, and other potentially sensitive data. They should not be committed to a public repository.

## 6. Firewall Configuration

The Ubuntu firewall was configured to restrict incoming connections.

The lab allowed the required TCP connections from the host-only network:

| Port | Protocol | Purpose |
|---|---|---|
| 443 | TCP | Dashboard access |
| 1514 | TCP | Agent event communication |
| 1515 | TCP | Agent enrollment |

Dashboard access on port `443` was restricted to the Windows host-only address.

Port `9200` was used for internal Manager-to-Indexer communication and was not intended to be exposed to the Windows host.

Connectivity was tested from Windows using PowerShell's `Test-NetConnection` command.

## 7. Configuration Validation

The following checks were completed:

- Windows agent service was running.
- Wazuh Dashboard showed the endpoint as active.
- FIM events appeared in the dashboard.
- Windows Security events reached the Manager.
- JSON archives contained endpoint events.
- Required network connections were successful.

## 8. Security and Repository Hygiene

Before publishing this project, configuration examples must be reviewed and sanitized.

Do not publish:

- Passwords or dashboard credentials
- Private keys or certificates
- Agent enrollment credentials
- Sensitive installation files
- Unfiltered raw event archives
- Personal information contained in logs

Only sanitized configuration examples should be included in the public repository.