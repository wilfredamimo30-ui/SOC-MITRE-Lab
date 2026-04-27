# SOC MITRE ATT&CK Home Lab

## Project Overview
A hands-on SOC analyst home lab simulating real cyberattacks and detecting them using Splunk SIEM.

## Lab Setup
- Attacker Machine: Kali Linux
- Victim Machine: Windows 10 (KVM Virtual Machine)
- SIEM: Splunk Enterprise on Kali Linux
- Log Forwarder: Splunk Universal Forwarder on Windows

## Techniques Simulated

### T1059 — Command Execution
- Simulated PowerShell reconnaissance commands
- Result: 114 events captured in Splunk

### T1110 — Brute Force
- Simulated password attack using Hydra from Kali
- Result: 2 failed logon events detected (EventCode 4625)

### T1078 — Valid Accounts
- Simulated attacker using valid credentials
- Result: 8 successful logon events detected (EventCode 4624)

## Tools Used
- Kali Linux
- Splunk Enterprise
- Splunk Universal Forwarder
- Hydra
- KVM/Virt-Manager

## Analyst: Wilfred Amimo
## Location: Nairobi, Kenya
## Date: April 2026
