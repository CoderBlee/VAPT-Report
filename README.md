# Vulnerability Assessment and Penetration Testing (VAPT) Report:
1. Introduction

This report documents the Vulnerability Assessment and Penetration Testing (VAPT) activities performed on a virtual machine to identify security vulnerabilities and assess the effectiveness of implemented security controls. The testing was conducted on a Debian-based virtual machine using Kali Linux as the attacking platform.
2. Objectives
The primary objectives of this VAPT exercise were to:
- Identify security vulnerabilities in the target virtual machine.
- Exploit identified vulnerabilities to determine the potential impact on the system.
- Provide recommendations to mitigate the discovered vulnerabilities.

3. Methodology

The testing methodology followed a structured approach, which included the following phases:
- Planning and Preparation: Setting up the testing environment.
- Discovery and Information Gathering: Identifying the target system’s network configuration and services.
- Vulnerability Identification: Scanning for vulnerabilities in the target system.
- Exploitation: Attempting to exploit identified vulnerabilities.
- Reporting: Documenting findings and providing mitigation recommendations.

4. Testing Environment

- Attacker System: Kali Linux
- Target System: Debian-based virtual machine
- Network Configuration: Bridged Adapter to ensure the target and attacker systems are on the same network.

5. Procedures and Findings

 5.1. Setting Up the Virtual Machine

1. Importing Appliance:
   - Imported the virtual machine using the OVF file.
   - Configured the virtual machine settings: General -> Version: Debian (64-bit), Network -> Bridged Adapter.

 5.2. Network Configuration

1. Kali Linux Network Setup:
   - Ensured the Kali Linux system was on the same network as the target system by configuring the network settings.

2. Network Discovery:
   - Used `ifconfig` to identify the network configuration.
   - Ran `sudo netdiscover` to identify live hosts on the network.

5.3. Service Enumeration

1. Identified Open Ports and Services:
   - IP Address: `192.168.43.77`
   - Services: 
     - `22/tcp` open ssh (OpenSSH 7.2p2)
     - `80/tcp` open http (GoDaddy net/http server)
     - `10080/tcp` open

5.4. Vulnerability Exploitation

1. HTTP Service (Port 80):
   - Accessed the web service hosted on `http://192.168.43.77:80`.

2. Bypassing Restricted Ports:
   - Modified Firefox settings to access the banned port `10080` by navigating to `about:config`, accepting the risk, and adding `10080` to the `network.security.ports.banned.override` string.

3. SQL Injection:
   - Attempted SQL Injection on the login page using the payload: `' OR '1'='1`.

 5.5. Post-Exploitation

1. Reverse Shell:
   - Established a reverse shell using Netcat:
     - Command: `nc -lvnp 4444`
     - Executed payload from Firefox: `http://127.0.0.1:8086/ScriptText`

2. Privilege Escalation:
   - Executed commands to gather system information:
     - `pwd`
     - `whoami`
     - `ls`
     - `cat /etc/passwd`
     - Executed `LinEnum.sh` for enumeration.

3. Sensitive Data Extraction:
   - Edited and viewed private key file:
     - `nano privatekey.txt`
     - Copied content, saved (`Ctrl+X -> y`), and viewed (`cat privatekey.txt`).

6. Recommendations

Based on the findings, the following recommendations are made to enhance the security posture of the target system:
- Network Segmentation: Isolate critical systems from less secure networks.
- Service Hardening: Ensure that unnecessary services are disabled, and necessary services are properly configured.
- Input Validation: Implement robust input validation to prevent SQL injection attacks.
- Port Restrictions: Properly configure firewalls to restrict access to sensitive ports.
- Regular Audits: Conduct regular security audits and vulnerability assessments to identify and mitigate potential security risks.

7. Conclusion

The VAPT exercise revealed several security vulnerabilities within the target virtual machine, including open ports with potentially exploitable services and vulnerabilities in web application logic. By addressing the identified vulnerabilities and implementing the recommended security measures, the overall security posture of the target system can be significantly improved.

8. References

- Kali Linux Documentation
- OWASP Testing Guide
- NIST Special Publication 800-115

