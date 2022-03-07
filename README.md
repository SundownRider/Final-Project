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

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Blue%20Team/Final_Project_Network.png" width=80% height=80%>

Kali Linux

- **Operating System**: Kali Linux
- **Purpose**: Designed to conduct a penetration test against Target 1 and Target 2
- **IP Address**: 192.168.1.90

ELK

- **Operating System**: Linux
- **Purpose**: Collects security and access logs from Target 1 and Target 2
- **IP Address**: 192.168.1.100

Capstone

- **Operating System**: Linux
- **Purpose**: A vulnerable VM that can be used to test security alerts
- **IP Address**: 192.168.1.105

Target 1

- **Operating System**: Linux (Ubuntu)
- **Purpose**: Vulnerable VM Running Wordpress
- **IP Address**: 192.168.1.110

Target 2

- **Operating System**: Linux (Ubuntu)
- **Purpose**: Vulnerable VM Running Wordpress
- **IP Address**: 192.168.1.115

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

![HTTP_Errors.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/053c197000e4464ca133fbae8b98da51.png)

#### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#name-of-alert-2)HTTP Request Size Monitor

HTTP Request Size Monitor is implemented as follows:

- **Metric**: HTTP Request Size
- **Threshold**: Is Above 3500
- **Vulnerability Mitigated**: HTTP Request Smuggling
- **Reliability**: Medium Reliability

![HTTP_Size.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/c28443daa3a243379affeec47e45d1c8.png)

#### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#name-of-alert-3)CPU Usage Monitor

CPU Usage Monitor is implemented as follows:

- **Metric**: CPU Usage
- **Threshold**: Is Above 0.5
- **Vulnerability Mitigated**: TODO
- **Reliability**: Medium Reliability

