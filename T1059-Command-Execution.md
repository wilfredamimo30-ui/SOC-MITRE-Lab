# T1059 — Command and Scripting Interpreter

## What Is This Attack?
T1059 is a MITRE ATT&CK technique where an attacker uses 
command line tools like PowerShell or CMD to run malicious 
commands on a victim machine. It is one of the most commonly 
used techniques in real cyberattacks because PowerShell is 
built into every Windows machine.

## Objective
Simulate an attacker who has gained access to a Windows machine 
and is running reconnaissance commands to gather information 
about the system.

## Environment
- Attacker Machine: Kali Linux
- Victim Machine: Windows 10 (192.168.122.24)
- Detection Tool: Splunk Enterprise on Kali Linux
- Log Forwarder: Splunk Universal Forwarder on Windows

## Commands Simulated
The following commands were run on the Windows victim machine 
using PowerShell as Administrator:

whoami
ipconfig
net user
net localgroup administrators
systeminfo

## Why These Commands?
These are standard reconnaissance commands an attacker runs 
after gaining access to a system to find out:
- Who they are logged in as
- What the network looks like
- What users exist on the machine
- Who has administrator privileges
- System information for further exploitation

## Detection Setup
- Enabled Process Creation Audit Policy via Group Policy
- Enabled Command Line logging via Registry
- Splunk Universal Forwarder collecting Windows Event Logs
- Splunk Enterprise receiving and indexing logs on Kali

## Splunk Search Used
index=main EventCode=4688

## Results
- Total Events Captured: 114
- EventCode: 4688 (Process Creation)
- Detection: Successful

## What EventCode 4688 Means
Event ID 4688 fires every time a new process is created on 
Windows. When an attacker runs PowerShell commands each command 
generates a 4688 event giving the SOC analyst a full trail 
of what was executed on the machine.

## What A Real SOC Analyst Would Do Next
1. Identify which user ran the commands
2. Check if the user should have PowerShell access
3. Look at parent process to see what spawned PowerShell
4. Check if commands match known attack patterns
5. Escalate to incident response if suspicious

## Challenges Faced
- Had to enable Process Creation audit policy manually
- Default Windows logging does not capture PowerShell commands
- Had to enable audit policy via Group Policy and Registry

## Conclusion
T1059 simulation was successful. Splunk detected and logged 
114 process creation events proving that the detection 
capability is working correctly.
