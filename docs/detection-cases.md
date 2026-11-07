# Detection Cases — Wazuh Mini SOC Lab

## 1. Overview

This document describes the security detection scenarios implemented and validated in the Wazuh Mini SOC Lab.

The tests focused on:

1. File Integrity Monitoring (FIM)
2. Windows failed-login detection
3. Windows account-related security events observed in the lab

The purpose was to generate endpoint activity, collect the resulting events, and verify that Wazuh could analyze and display the corresponding alerts.

---

## 2. Detection Case 1 — File Creation

**Objective:** Detect the creation of a new file in a monitored Windows directory.

**Monitored directory:**

`C:\Wazuh-FIM-Test`

**Test procedure:**

1. Open the monitored directory on Windows.
2. Create a new test file.
3. Allow the Wazuh agent to collect the filesystem event.
4. Search for the resulting alert in the Wazuh Dashboard.

**Observed result:**

- Wazuh detected the newly created file.
- Rule ID: `554`
- Description: File added.
- Severity level: 5.

**Outcome:** Successful.

---

## 3. Detection Case 2 — File Modification

**Objective:** Detect changes to an existing file in the monitored directory.

**Test procedure:**

1. Open an existing test file.
2. Modify its contents.
3. Save the file.
4. Review the resulting Wazuh alert.

**Observed result:**

- Wazuh detected a checksum change.
- Rule ID: `550`
- Severity level: 7.
- Associated MITRE ATT&CK technique: `T1565.001`.

**Outcome:** Successful.

---

## 4. Detection Case 3 — File Deletion

**Objective:** Detect the deletion of a file from the monitored directory.

**Test procedure:**

1. Select an existing test file.
2. Delete the file.
3. Review the Wazuh Dashboard for the corresponding alert.

**Observed result:**

- Wazuh detected the file deletion.
- Rule ID: `553`
- Severity level: 7.
- Associated MITRE ATT&CK techniques: `T1070.004` and `T1485`.

**Outcome:** Successful.

---

## 5. Detection Case 4 — Failed Windows Login

**Objective:** Detect an unsuccessful authentication attempt on the Windows endpoint.

**Test procedure:**

1. Use the dedicated test account `SOC-Test`.
2. Attempt authentication with an intentionally incorrect password.
3. Confirm that Windows rejects the authentication attempt.
4. Verify that Windows generates Security Event ID `4625`.
5. Check the Wazuh Dashboard for the corresponding detection.

**Observed Windows event:**

- Event ID: `4625`
- Event description: An account failed to log on.
- The authentication attempt was rejected because of an incorrect password.

**Observed Wazuh alert:**

- Rule ID: `60122`
- Description: Logon Failure - Unknown user or bad password.
- Severity level: 5.
- Agent: `Windows10-Endpoint`
- Agent ID: `001`
- Number of matching alerts observed: 2.

**Outcome:** Successful. The failed-login events were collected by the Wazuh agent, analyzed by the Manager, and displayed in the Dashboard.

---

## 6. Additional Observation — Account-Related Events

During lab monitoring, account-related Windows security events were also observed.

The following Wazuh rules appeared in the alert data:

| Rule ID | Observed description |
|---|---|
| 60109 | User account enabled or created |
| 60110 | User account changed |

These events demonstrate that the lab collected account-related security activity.

**Important:** These are documented as observed events, not as independently validated detection tests.

---

## 7. Detection Summary

| Detection scenario | Wazuh rule ID | Severity | Result |
|---|---:|---:|---|
| File creation | 554 | 5 | Successful |
| File modification | 550 | 7 | Successful |
| File deletion | 553 | 7 | Successful |
| Failed Windows login | 60122 | 5 | Successful |
| Account-related events | 60109, 60110 | Not specified | Observed |

## 8. Conclusion

The Wazuh Mini SOC Lab successfully demonstrated endpoint monitoring and security-event detection.

The validated scenarios covered filesystem changes and failed authentication attempts. The Dashboard provided visibility into the generated alerts, including their rule IDs and severity levels.

These tests demonstrate practical experience with endpoint monitoring, log collection, security-event analysis, and basic threat detection using Wazuh.