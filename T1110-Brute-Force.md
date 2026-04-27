# T1110 — Brute Force Attack

## What Is This Attack?
T1110 is a MITRE ATT&CK technique where an attacker repeatedly 
tries different passwords until they find the correct one. 
This is called a brute force attack. It is commonly used 
against login services like RDP, SSH, and SMB.

## Objective
Simulate an attacker trying to break into the Windows victim 
machine by repeatedly attempting different passwords against 
the Administrator account using Hydra from Kali Linux.

## Environment
- Attacker Machine: Kali Linux (192.168.122.1)
- Victim Machine: Windows 10 (192.168.122.24)
- Detection Tool: Splunk Enterprise on Kali Linux
- Log Forwarder: Splunk Universal Forwarder on Windows
- Attack Tool: Hydra

## Attack Simulation
Hydra was used from Kali Linux to simulate a brute force 
attack against the Windows Administrator account over RDP.

Command used:
hydra -l administrator -P /usr/share/wordlists/rockyou.txt 
192.168.122.24 rdp

## Why RDP?
Remote Desktop Protocol is one of the most commonly attacked 
services in real world cyberattacks. Attackers target RDP 
because it gives full remote access to a machine if 
credentials are found.

## Detection Setup
- Enabled Logon Audit Policy on Windows
- Splunk Universal Forwarder collecting Security Event Logs
- Splunk Enterprise receiving and indexing logs on Kali Linux

## Splunk Search Used
index=main EventCode=4625

## Results
- Total Failed Logon Events Captured: 2
- EventCode: 4625 (Failed Logon Attempt)
- Detection: Successful

## What EventCode 4625 Means
Event ID 4625 fires every time a failed login attempt occurs 
on Windows. During a brute force attack each failed password 
attempt generates a 4625 event giving the SOC analyst 
visibility into the attack.

## What A Real SOC Analyst Would Do Next
1. Count the number of failed logons in a short time period
2. Identify the source IP of the attack
3. Check if the same IP is targeting multiple accounts
4. Block the source IP at the firewall
5. Alert the incident response team

## Challenges Faced
- SMB service was not available on Windows VM
- Had to enable RDP manually via registry
- Had to enable Logon Audit Policy to capture failed logons
- Default Windows logging does not capture failed logons

## Conclusion
T1110 brute force simulation was successful. Splunk detected 
and logged 2 failed logon events proving that the detection 
capability is working correctly. In a real environment 
hundreds of failed logons in a short time would trigger 
an immediate alert.
