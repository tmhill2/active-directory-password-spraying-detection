# Active Directory Password Spraying Detection Lab

## Overview
This project simulates a password spraying attack against an Active Directory domain and detects it using Splunk.

## Lab Environment
- Windows 10 (Domain Joined)
- Windows Server (Domain Controller - SOC-SRV01)
- Splunk Enterprise
- Splunk Universal Forwarder
- VirtualBox Host-Only Network

## Attack Simulation
Simulated password spraying using repeated failed SMB authentication attempts.

## Attack Execution

The following command was used to simulate a password spraying attack against the Domain Controller:

```cmd
for /L %i in (1,1,15) do net use \\SOC-SRV01\IPC$ /user:SOCLAB\sprayuser%i WrongPass123
```

![Attack Execution](screenshots/01-attack-execution.png)

## Failed Logon Events (Event ID 4625)

The simulated password spray generated multiple failed network authentication events (Event ID 4625) on the Domain Controller.

![Failed Logon Events](screenshots/02-4625-events.png)

## Detection Query

To detect password spraying behavior, distinct failed usernames were counted per workstation:

```spl
index=wineventlog host=SOC-SRV01 EventCode=4625 Logon_Type=3
| stats dc(Account_Name) as unique_users by Workstation_Name
```

The query identifies a high number of unique failed logon attempts originating from a single workstation.

![Detection Query Results](screenshots/03-detection-query.png)

## Alert Configuration

A scheduled Splunk alert was created to detect password spraying activity when more than 10 unique failed network logons (Event ID 4625) occur from a single workstation.

```spl
index=wineventlog host=SOC-SRV01 EventCode=4625 Logon_Type=3
| stats dc(Account_Name) as unique_users by Workstation_Name
| where unique_users > 10
```

![Alert Logic](screenshots/04-alert-logic.png)

![Alert Configuration](screenshots/05-alert-configuration.png)

## Incident Summary

A simulated password spraying attack was conducted against the Active Directory domain using repeated SMB authentication attempts.

The attack generated multiple failed logon events (Event ID 4625) on the Domain Controller. Splunk successfully ingested the logs via the Universal Forwarder.

Detection logic was developed to identify abnormal authentication behavior by counting distinct failed usernames per workstation. When the threshold exceeded 10 unique failed logons within the defined time window, a scheduled alert triggered.

This project demonstrates:
- Active Directory attack simulation
- Windows Event Log analysis
- SPL detection engineering
- Threshold-based alerting
- MITRE ATT&CK mapping (T1110.003)




