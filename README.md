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
for /L %i in (1,1,15) do net use \\SOC-SRV01\IPC$ /user:SOCLAB\sprayuser%i WrongPass123'''




## Detection Strategy
Detected abnormal volume of failed network logons across multiple user accounts from a single workstation.

## MITRE ATT&CK Mapping
T1110.003 – Password Spraying

---
Screenshots and detection details below.
