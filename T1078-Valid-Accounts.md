# T1078 — Valid Accounts

## What Is This Attack?
T1078 is a MITRE ATT&CK technique where an attacker uses 
legitimate credentials to log into a system. This is dangerous 
because the attacker blends in with normal user activity 
making it harder to detect than other attack types.

## Objective
Simulate an attacker who has obtained valid credentials and 
uses them to log into the Windows victim machine without 
raising suspicion.

## Environment
- Attacker Machine: Kali Linux (192.168.122.1)
- Victim Machine: Windows 10 (192.168.122.24)
- Detection Tool: Splunk Enterprise on Kali Linux
- Log Forwarder: Splunk Universal Forwarder on Windows

## Attack Simulation
A new user account called attacker was created on the Windows 
victim machine and added to the administrators group to 
simulate a compromised privileged account.

Commands used on Windows:
net user attacker Password123! /add
net localgroup administrators attacker /add

The attacker account was then used to open a new CMD session 
simulating malicious use of valid credentials:
runas /user:attacker cmd

## Why This Technique Is Dangerous
Unlike brute force attacks valid account usage looks completely 
normal to basic security tools. The attacker is using real 
credentials so there are no failed login attempts to alert on. 
This is why context and behaviour analysis is critical in SOC 
work.

## Detection Setup
- Enabled Logon Audit Policy on Windows
- Splunk Universal Forwarder collecting Security Event Logs
- Splunk Enterprise receiving and indexing logs on Kali Linux

## Splunk Search Used
index=main EventCode=4624

## Results
- Total Successful Logon Events Captured: 8
- EventCode: 4624 (Successful Logon)
- Detection: Successful

## What EventCode 4624 Means
Event ID 4624 fires every time a successful login occurs on 
Windows. By monitoring these events and looking for unusual 
accounts logging in at unusual times a SOC analyst can detect 
T1078 activity.

## What A Real SOC Analyst Would Do Next
1. Check if the account that logged in is expected
2. Look at what time the login happened
3. Check what the account did after logging in
4. Compare against baseline normal behaviour
5. Investigate if a new admin account was recently created
6. Escalate to incident response if suspicious

## Challenges Faced
- RDP connection from Kali failed due to certificate issues
- Used runas command as alternative simulation method
- Valid account attacks are harder to detect without 
  behavioural baselines

## Conclusion
T1078 valid account simulation was successful. Splunk detected 
and logged 8 successful logon events proving that the detection 
capability is working. In a real environment a new admin 
account logging in unexpectedly would trigger an immediate 
investigation.
