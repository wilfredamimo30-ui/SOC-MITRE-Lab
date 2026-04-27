# SOC Lab Findings Summary

## Project Overview
This lab simulated three real cyberattack techniques from the 
MITRE ATT&CK framework on a controlled Windows victim machine 
and measured the detection capability of Splunk SIEM.

## Analyst
Name: Wilfred Amimo
Role: Cybersecurity Professional — SOC Analyst
Location: Nairobi, Kenya
Date: April 2026

## Lab Environment
- Attacker Machine: Kali Linux
- Victim Machine: Windows 10 KVM Virtual Machine
- SIEM: Splunk Enterprise on Kali Linux
- Log Forwarder: Splunk Universal Forwarder on Windows
- Network: Host Only — isolated from internet

## Results Summary

| Technique | ID | Events Detected | EventCode | Result |
|---|---|---|---|---|
| Command Execution | T1059 | 114 | 4688 | Detected |
| Brute Force | T1110 | 2 | 4625 | Detected |
| Valid Accounts | T1078 | 8 | 4624 | Detected |

## Key Findings

### Finding 1 — Default Windows Logging Is Not Enough
Windows does not log process creation or failed logons by 
default. Audit policies had to be manually enabled via 
Group Policy and Registry before Splunk could detect attacks. 
This is a critical gap in many real world environments.

### Finding 2 — T1059 Generated The Most Noise
Command execution generated 114 events — the highest of all 
three techniques. This shows that PowerShell activity is 
highly visible when logging is properly configured making 
it easier to detect than other techniques.

### Finding 3 — T1078 Is The Hardest To Detect
Valid account usage generated successful logon events which 
look identical to normal user activity. Without behavioural 
baselines and user activity monitoring this technique can 
easily go unnoticed in a real environment.

### Finding 4 — T1110 Generated The Least Events
Only 2 failed logon events were captured during the brute 
force simulation. In a real attack hundreds of failed logons 
would occur in seconds making it easier to detect through 
threshold based alerting.

## Challenges Faced During Setup
- VMware Workstation broke after Kali Linux kernel update
- Migrated entire lab from VMware to KVM
- EFI vs BIOS firmware conflicts on Windows VM
- Splunk Enterprise and Universal Forwarder port conflicts
- Windows disk space ran out during installation
- Had to manually enable multiple audit policies
- RDP connection issues during T1110 simulation

## Lessons Learned
1. Proper audit policy configuration is essential for detection
2. Default Windows settings are not SOC ready
3. SIEM placement matters — Splunk on Kali receiving from 
   Windows forwarder is the correct professional setup
4. Every error in a lab environment teaches something that 
   a classroom never could
5. Documentation is as important as the technical work itself

## Tools Used
- Kali Linux
- KVM and Virt-Manager
- Splunk Enterprise
- Splunk Universal Forwarder
- Hydra
- PowerShell
- Windows Group Policy Editor
- MITRE ATT&CK Framework

## Next Steps
- Set up automated alerting in Splunk for each EventCode
- Simulate more MITRE ATT&CK techniques
- Add more victim machines to the lab
- Practice log correlation across multiple event types
- Build Splunk dashboards for real time monitoring

## Conclusion
All three MITRE ATT&CK techniques were successfully simulated 
and detected. This lab proves that with proper configuration 
Splunk can effectively detect common attack techniques used 
in real world environments. The challenges faced during setup 
mirror real world SOC environments where things rarely work 
perfectly and troubleshooting is a core skill.
