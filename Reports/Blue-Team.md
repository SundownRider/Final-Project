# Blue Team: Summary of Operations

## [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#table-of-contents)Table of Contents

- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### [](https://github.com/the-Coding-Boot-Camp-at-UT/UTA-VIRT-CYBER-PT-09-2021-U-LOL/blob/master/1-Lesson-Plans/24-Final-Project/Resources/DefensiveTemplate.md#network-topology)Network Topology

The following machines were identified on the network:

<img src="https://github.com/SundownRider/Final-Project/blob/main/Images/Blue-Team/Final_Project_Network.png" width=50% height=50%>

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
