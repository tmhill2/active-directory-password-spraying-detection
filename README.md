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

## Detection Strategy
Detected abnormal volume of failed network logons across multiple user accounts from a single workstation.

## MITRE ATT&CK Mapping
T1110.003 – Password Spraying

---
Screenshots and detection details below.
