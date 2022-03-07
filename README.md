# Final-Project

# Red Team: Summary of Operations

## [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/OffensiveTemplate.md#table-of-contents)Table of Contents

- Exposed Services
- Critical Vulnerabilities
- Exploitation

### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/OffensiveTemplate.md#exposed-services)Exposed Services

Nmap scan results for each machine reveal the below services and OS details:

Nmap Command: nmap -sT -A -p- -Pn -T4 --reason -oA host-1-scan 192.168.1.110

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_1.png" width=60% height=60%>

This scan identifies the services below as potential points of entry:

- Target 1

Potentially Vulnerable Services:

1. SSH (Port 22)

2. HTTP (Port 80)

3. SMB (Port 445)

The following vulnerabilities were identified on each target:

- Target 1
    - CWE-521: Weak Password Requirements
    - CWE-732: Incorrect Permission Assignment For Critical Resource
    - CWE-522: Insufficiently Protected Credentials
    - CWE-250: Execution With Unnecessary Privileges

### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/OffensiveTemplate.md#exploitation)Exploitation

The Red Team was able to penetrate Target 1 and retrieve the following confidential data:

- Target 1
    - Flag 1: b9bbcb33e11b80be759c4e844862482d
        - Flag 1 Location: /var/www/html/service.html
        - Exploit Method: Logging Into The Machine Using Weak Login Credentials (michael:michael)
        - CWE-521: Weak Passwords Requirements
        - Command: ssh michael@192.168.1.110
        
        <img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Flag_1.png" width=50% height=50%>
        
    - Flag 2: fc3fd58dcdad9ab23faca6e9a36e581c
        - Flag 2 Location: /var/www/flag2.txt
        - Exploit Method: Navigating To The Directory Prior To The Apache Web Root
        - CWE-732: Incorrect Permission Assignment For Critical Resource
        - Command: find / -type f -iname *flag*
        
    - Flag 3:
        - Flag 3 Location: MySQL Database -> Wordpress Database -> Wordpress Posts Table
        - Exploit: Accessing The Database
        - CWE-522: Insufficiently Protected Credentials
        - Command: cat /var/www/html/wordpress/config.php (Database Credentials - root:R@v3nSecurity)
        
    - Flag 4:
        - Flag 4 Location: /root
        - Exploit: Gaining Root Access By Spawning A System Shell Using Sudo And Python
        - CWE-250: Execution With Unnecessary Privileges
        - Command: sudo python -c 'import os; os.spawn("/bin/bash")

# Blue Team: Summary of Operations

## [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#table-of-contents)Table of Contents

- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#network-topology)Network Topology

The following machines were identified on the network:

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Blue%20Team/Final_Project_Network.png" width=50% height=50%>

Kali Linux

- **Operating System**: Kali Linux
- **Purpose**: Designed to conduct a penetration test against Target 1
- **IP Address**: 192.168.1.90

ELK

- **Operating System**: Linux
- **Purpose**: Collects security and access logs from Target 1
- **IP Address**: 192.168.1.100

Capstone

- **Operating System**: Linux
- **Purpose**: A vulnerable VM that can be used to test security alerts
- **IP Address**: 192.168.1.105

Target 1

- **Operating System**: Linux (Ubuntu)
- **Purpose**: Vulnerable VM Running Wordpress
- **IP Address**: 192.168.1.110

### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#description-of-targets)Description of Targets

Target 1 (192.168.1.110)

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#monitoring-the-targets)Monitoring the Targets

Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:

#### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#name-of-alert-1)Excessive HTTP Errors

Excessive HTTP Errors is implemented as follows:

- **Metric**: HTTP Errors
- **Threshold**: Is Above 400
- **Vulnerability Mitigated**: Directory Brute Forcing
- **Reliability**: High Reliability

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Blue%20Team/HTTP_Errors.png" width=60% height=60%>

#### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#name-of-alert-2)HTTP Request Size Monitor

HTTP Request Size Monitor is implemented as follows:

- **Metric**: HTTP Request Size
- **Threshold**: Is Above 3500
- **Vulnerability Mitigated**: HTTP Request Smuggling
- **Reliability**: Medium Reliability

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Blue%20Team/HTTP_Size.png" width=60% height=60%>

#### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#name-of-alert-3)CPU Usage Monitor

CPU Usage Monitor is implemented as follows:

- **Metric**: CPU Usage
- **Threshold**: Is Above 0.5
- **Vulnerability Mitigated**: TODO
- **Reliability**: Medium Reliability

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Blue%20Team/CPU.png" width=60% height=60%>

# Network Analysis

## Time Thieves

At least two users on the network have been wasting time on YouTube. Usually, IT wouldn't pay much mind to this behavior, but it seems these people have created their own web server on the corporate network. So far, Security knows the following about these time thieves:

- They have set up an Active Directory network.
    
- They are constantly watching videos on YouTube.
    
- Their IP addresses are somewhere in the range 10.6.12.0/24.
    

You must inspect your traffic capture to answer the following questions:

1. What is the domain name of the users' custom site?

**Answer:** DESKTOP-86J4BX.frank-n-ted.com (10.6.12.157)

2. What is the IP address of the Domain Controller (DC) of the AD network?

**Answer:** Frank-n-Ted-DC.frank-n-ted.com (10.6.12.12)

3. What is the name of the malware downloaded to the 10.6.12.203 machine? Once you have found the file, export it to your Kali machine's desktop.

**Answer:** june11.dll

4. Upload the file to [VirusTotal.com](https://www.virustotal.com/gui/). What kind of malware is this classified as?

**Answer:** Trojan

## Vulnerable Windows Machines

The Security team received reports of an infected Windows host on the network. They know the following:

- Machines in the network live in the range 172.16.4.0/24.
    
- The domain mind-hammer.net is associated with the infected computer.
    
- The DC for this network lives at 172.16.4.4 and is named Mind-Hammer-DC.
    
- The network has standard gateway and broadcast addresses.
    

Inspect your traffic to answer the following questions:

1. Find the following information about the infected Windows machine:

**Host name:** Rotterdam-PC

**IP address:** 172.16.4.205

**MAC address:** 00:59:07:b0:63:a4

2. What is the username of the Windows user whose computer is infected?

**Username:** matthijs.devries

3. What are the IP addresses used in the actual infection traffic?

**IP Address:** 185.243.115.84

## Illegal Downloads

IT was informed that some users are torrenting on the network. The Security team does not forbid the use of torrents for legitimate purposes, such as downloading operating systems. However, they have a strict policy against copyright infringement.

IT shared the following about the torrent activity:

- The machines using torrents live in the range 10.0.0.0/24 and are clients of an AD domain.
    
- The DC of this domain lives at 10.0.0.2 and is named DogOfTheYear-DC.
    
- The DC is associated with the domain dogoftheyear.net.
    

Your task is to isolate torrent traffic and answer the following questions:

1.  Find the following information about the machine with IP address 10.0.0.201:

**Hostname:** BLANCO-DESKTOP

**MAC Address:** 00:16:17:18:66:c8

