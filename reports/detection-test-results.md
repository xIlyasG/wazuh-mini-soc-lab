# Detection Test Results Report

## 1. Project Information

| Field | Details |
|---|---|
| Project | Wazuh Mini SOC Lab |
| Wazuh version | 4.14.10 |
| Operating system | Ubuntu 26.04.1 LTS |
| Monitored endpoint | Windows 10 host |
| Agent name | Windows10-Endpoint |
| Agent ID | 001 |
| Test date | November 7, 2026 |

## 2. Executive Summary

The Wazuh Mini SOC Lab was tested to validate endpoint monitoring, file integrity detection, and Windows authentication-event monitoring.

The tests confirmed that Wazuh successfully detected file creation, modification, deletion, and failed Windows authentication attempts.

The resulting alerts were received and displayed in the Wazuh Dashboard.

## 3. Test Results

| Test | Event / Rule ID | Severity | Result |
|---|---|---|---|
| File creation | Rule 554 | 5 | PASS |
| File modification | Rule 550 | 7 | PASS |
| File deletion | Rule 553 | 7 | PASS |
| Failed Windows login | Event 4625 / Rule 60122 | 5 | PASS |
| Account-related events | Rules 60109, 60110 | Not specified | OBSERVED |

## 4. Detailed Results

### 4.1 File Creation

- **Action:** Created a file in `C:\Wazuh-FIM-Test`.
- **Expected result:** Wazuh generates a file-added alert.
- **Observed result:** Rule 554 detected the new file.
- **Status:** PASS.

### 4.2 File Modification

- **Action:** Modified the contents of a monitored file.
- **Expected result:** Wazuh detects a checksum change.
- **Observed result:** Rule 550 generated a checksum-change alert.
- **Severity:** 7.
- **Status:** PASS.

### 4.3 File Deletion

- **Action:** Deleted a file from the monitored directory.
- **Expected result:** Wazuh detects the deletion.
- **Observed result:** Rule 553 generated a file-deleted alert.
- **Severity:** 7.
- **Status:** PASS.

### 4.4 Failed Windows Authentication

- **Action:** Attempted authentication using an intentionally incorrect password for the dedicated test account.
- **Expected result:** Windows records a failed logon and Wazuh detects it.
- **Windows Event ID:** 4625.
- **Wazuh Rule ID:** 60122.
- **Wazuh description:** Logon Failure - Unknown user or bad password.
- **Severity:** 5.
- **Observed matching alerts:** 2.
- **Status:** PASS.

The failed authentication events were verified in the Windows Security log, Wazuh archives, and Wazuh Dashboard.

### 4.5 Account-Related Events

- **Observed rules:** 60109 and 60110.
- **Activity:** Account-related security events.
- **Status:** OBSERVED.

These events were visible in the alert data but were not treated as independently validated test cases.

## 5. Evidence

Screenshots should be stored in the repository's `screenshots` directory.

Suggested evidence:

| Evidence filename | Description |
|---|---|
| `fim-file-creation.png` | Windows monitored directory after file creation |
| `fim-file-modification.png` | Wazuh checksum-change alert |
| `fim-file-deletion.png` | Wazuh file-deletion alert |
| `failed-login-event-4625.png` | Windows Security failed-login event |
| `failed-login-wazuh-alert.png` | Wazuh rule 60122 in the Dashboard |
| `windows-agent-active.png` | Windows endpoint shown as active |

Only screenshots that were actually captured should be included.

Before publishing screenshots, remove or obscure passwords, tokens, private keys, personal information, and other sensitive details.

## 6. Overall Assessment

**Validated detection scenarios:** 4

**Successful detection tests:** 4

**Additional observations:** Account-related events under rules 60109 and 60110.

The results demonstrate that the lab can collect Windows endpoint security events, detect filesystem changes, identify failed authentication attempts, and present security alerts through a centralized dashboard.

## 7. Limitations

- This is a small educational lab, not a production SOC deployment.
- The environment uses one Wazuh server and one monitored Windows host.
- Ubuntu 26.04.1 LTS was not listed among the officially supported operating systems for Wazuh 4.14.10 at implementation time.
- Detection results are limited to the scenarios tested in this lab.

## 8. Conclusion

The detection tests were successful. The lab provided practical experience with Wazuh deployment, endpoint enrollment, File Integrity Monitoring, Windows Security event collection, and security alert investigation.