# Data Exfiltration and Remediation
This project simulates a real-world scenario where an attacker exploits a misconfigured FTP server to exfiltrate sensitive data. It walks through the attack process using Kali Linux and Nmap, and then demonstrates how a SOC analyst can detect and respond using practical remediation steps.

Project Overview

Goal: Detect and remediate unauthorized FTP data exfiltration.


Problem Statement
Company X is facing a data breach from an exposed FTP server. The attacker exploits misconfigurations, using weak credentials to gain unauthorized access and exfiltrate client contracts, risking $500K in fines. The SOC team must detect and stop this to prevent further leakage and protect business assets like intellectual property.

Lab setup
• Virtualization: VirtualBox (running lab on VirtualMachines)

• Attack Tools: Kali Linux (simulates attacker)

• Target System: Windows 10 (hosts FTP target)

• Network Scanning: Nmap (detects FTP vulnerabilities)

• Server: FileZilla (simulates vulnerable FTP)


Step-by-Step Execution

Step 1: Setting Up VirtualBox.

Goal: Establish a virtual environment for testing.

• Download VirtualBox from here [https://www.virtualbox.org/wiki/Downloads].

• Install VirtualBox—double-click exe > Next > Next > Install > Finish.

• Verify by opening VirtualBox.




Screenshot: VirtualBox installed successfully.


Step 2: Downloading & Installing OS Images

Goal: Ensure Kali Linux (attacker) and Windows 10 (victim) images are installed 

• Download Kali Linux VM from Kali Linux [https://www.kali.org/get-kali/]

• Download Windows 10 ISO from Microsoft [https://www.microsoft.com/en-us/software-download/windows10ISO]




Screenshot: Kali Linux & Windows 10 ISOs ready.



Step 3: Setting Up Kali Linux VM (Attacker Machine)

• Open VirtualBox, click File > Import Appliance, select Kali file.

• Adjust settings—RAM: 2048MB, Network: NAT—then Import.

• Start Kali VM and log in with username and password

• Update Kali and install Nmap: sudo apt update && sudo apt install nmap

• Confirm using this command : nmap -V to show version.




Screenshot: Kali VM running with Nmap installed.


Step 4: Setting Up Windows 10 VM (Victim Machine)

• Create a new Windows 10 VM in VirtualBox.

• Allocate the necessary settings.

• Attach the Windows 10 ISO and install.

• Set up user account.

• Check the Windows 10 IP: cmd ipconfig




Screenshot: Windows 10 VM configured.


Step 5: Installing & Configuring FTP Server on Windows 10

Goal: Simulate a vulnerable FTP setup.

• Download and install FileZilla Server.

• Set Port: 21 and connect.

• Create a new user with full access to a folder.

• Allow anonymous login and set a weak password.

• Check if FTP is running: cmd netstat -an | find "21"



Screenshot: FileZilla server set up.



Screenshot: FTP server running on port 21.


Step 6: Scanning the Network for FTP Vulnerabilities

Goal: Verify the exposure of the FTP server.

• On Kali, scan the network to detect FTP: nmap -sS -p 21 192.168.1.10

• Check for anonymous access: nmap --script ftp-anon 192.168.1.10



Screenshot: Scan the windows network for detection of FTP


Step 7: Exploiting FTP Weakness (Simulated Attack)

Goal: Demonstrate unauthorized access to the FTP server.

• Connect to FTP from Kali: ftp 192.168.1.10

• Bruteforce with Hydra: hydra -l admin -P wordlist.txt ftp://192.168.1.10

• Login with weak credentials: Username: admin, Password: password1

• List files: dir

• Download sensitive files: get confidential.docx


Screenshot: Wordlist of weak passwords



Screenshot: Successful data exfiltration and login.


Step 8: SOC Remediation & Fixes

Goal: Take direct action to stop the ongoing attack and secure the vulnerable FTP setup.

• Disable the FileZilla FTP Service:
  Set-Service -Name "FileZilla Server" -StartupType Disabled
  Stop-Service -Name "FileZilla Server"

• Block Port 21 Using Windows Firewall:
  Use the New-NetFirewallRule command to block incoming FTP traffic:
  New-NetFirewallRule -DisplayName "Block FTP" -Direction Inbound -LocalPort 21 -Protocol TCP -Action Block

• Disable Anonymous Login in FileZilla:
  Open FileZilla Server interface → Navigate to Edit > Users → Uncheck "Allow Anonymous Login"

• Update Weak Credentials:
  Either remove the user or enforce a stronger password policy with long, complex passwords.

Analyze Access Logs & Monitor Events:
Review FileZilla logs located at: C:\Program Files\FileZilla Server\Logs
Open Windows Event Viewer → Check Security and Application logs for suspicious login attempts and connection history.

Document & Report the Incident:
Record details such as attacker’s IP, timestamps, affected files, and the actions taken to stop the breach for proper documentation and escalation.

Screenshot: Disabled the FileZilla FTP service


Screenshot: Block port 21 using windows firewall



SOC Perspective: Identifying the Attack Process
Attacker's Steps:
• Reconnaissance: Scanned network with Nmap.

• Exploitation: Logged into FTP using weak credentials.

• Exfiltration: Downloaded files via FTP.



Business Impact & Recommendations

Impact of Data Exfiltration:
• Data Breach: Exposure of confidential files.

• Compliance Violations: GDPR, HIPAA, etc.

• Reputation Damage: Loss of customer trust.

Recommended Fixes:
1. Enforce Strong Passwords: Minimum 12 characters with complexity.

2. Restrict FTP Access: Allow only specific IPs.

3. Use Secure Protocols: Migrate to SFTP/FTPS.

4. Monitor Network Traffic logs: Use Splunk for FTP activity tracking.

5. Implement MFA: Prevent unauthorized access.

Personal Review & Lessons Learned
This project boosted my SOC skills by simulating real-world FTP attacks and defences. 
Identifying weak configurations and mitigating them from reconnaissance to remediation shows proactive security. 

Key takeaways:
1.Weak passwords are a major security gap.

2. Network monitoring’s key for spotting issues early on.

3. Simple fixes like disabling anonymous login can stop big breaches.

Next Steps...
Automate detection using Python scripts.

Build a Splunk dashboard for FTP logs.


