# writeup level00


# Enumeration

## What is enumeration: 
Enumeration forms the basis of information gathering of the target system during a cyber attack. Once attackers have established a connection with the target host during an enumeration attack, they can send directed queries to extract information on system vulnerabilities. Attackers typically assess attack vectors by leveraging the enumeration’s outputs to exploit the system further.In our case we have physical access.

What we know in this machine is user: level00 with password level00


Let Start some enumeration :
- whoami/id
level00@SnowCrash:~$ id
uid=2000(level00) gid=2000(level00) groups=2000(level00),100(users)



- what  is this machine:
from uname and lscpu we know that ubuntu 32bit with old(End of Lif) virsion ubuntu : ubuntu 12.04 precise pangolinI t was officially released on April 26, 2012

![uname/lscpu](https://cdn.discordapp.com/attachments/1164485225875783701/1164485411192721488/image.png?ex=6543628d&is=6530ed8d&hm=578e0fbcad7a73dac3e72469b3d6387075dd77f642d34821a096ff573c936696&)

source: https://askubuntu.com/questions/444394/what-is-the-meaning-of-i686-in-ubuntu

let list all users in /etc/passwd
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
syslog:x:101:103::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
whoopsie:x:103:107::/nonexistent:/bin/false
landscape:x:104:110::/var/lib/landscape:/bin/false
sshd:x:105:65534::/var/run/sshd:/usr/sbin/nologin
level00:x:2000:2000::/home/user/level00:/bin/bash
level01:x:2001:2001::/home/user/level01:/bin/bash
level02:x:2002:2002::/home/user/level02:/bin/bash
level03:x:2003:2003::/home/user/level03:/bin/bash
level04:x:2004:2004::/home/user/level04:/bin/bash
level05:x:2005:2005::/home/user/level05:/bin/bash
level06:x:2006:2006::/home/user/level06:/bin/bash
level07:x:2007:2007::/home/user/level07:/bin/bash
level08:x:2008:2008::/home/user/level08:/bin/bash
level09:x:2009:2009::/home/user/level09:/bin/bash
level10:x:2010:2010::/home/user/level10:/bin/bash
level11:x:2011:2011::/home/user/level11:/bin/bash
level12:x:2012:2012::/home/user/level12:/bin/bash
level13:x:2013:2013::/home/user/level13:/bin/bash
level14:x:2014:2014::/home/user/level14:/bin/bash
flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
flag02:x:3002:3002::/home/flag/flag02:/bin/bash
flag03:x:3003:3003::/home/flag/flag03:/bin/bash
flag04:x:3004:3004::/home/flag/flag04:/bin/bash
flag05:x:3005:3005::/home/flag/flag05:/bin/bash
flag06:x:3006:3006::/home/flag/flag06:/bin/bash
flag07:x:3007:3007::/home/flag/flag07:/bin/bash
flag08:x:3008:3008::/home/flag/flag08:/bin/bash
flag09:x:3009:3009::/home/flag/flag09:/bin/bash
flag10:x:3010:3010::/home/flag/flag10:/bin/bash
flag11:x:3011:3011::/home/flag/flag11:/bin/bash
flag12:x:3012:3012::/home/flag/flag12:/bin/bash
flag13:x:3013:3013::/home/flag/flag13:/bin/bash
flag14:x:3014:3014::/home/flag/flag14:/bin/bash
```

we know now the road to flag01 ;)

our target now is flog00 as subject say

we alaso have loock to /var dir if any mails/logs/websites/backups
![var](https://cdn.discordapp.com/attachments/691741851648262195/1163412552911437986/image.png?ex=653f7b5f&is=652d065f&hm=be5027a7fb9efd78153f82d170187678a5705bebf2d6be6e591be7f906a14553&)

And now we also know the other paths to different levels.

let check files created files by user flag00,we found :

```
level00@SnowCrash:~$ find / -user flag00 -exec ls -la {} \; -exec cat {} \; 2>/dev/null
----r--r-- 1 flag00 flag00 15 Mar  5  2016 /usr/sbin/john
cdiiddwpgswtgt
----r--r-- 1 flag00 flag00 15 Mar  5  2016 /rofs/usr/sbin/john
cdiiddwpgswtgt
```
we have read permission  on that /usr/sbin/john and in rofs(Read-only file System) that contain ciphertext: cdiiddwpgswtgt
After brute force to decrypt we found that Rot11 (Rotate by 11 Places)) use to enrypte 

cdiiddwpgswtgt ==> nottoohardhere

![cypher](https://cdn.discordapp.com/attachments/1015186220227231825/1164219253776470107/cypher.PNG?ex=65426aac&is=652ff5ac&hm=e55116545d274442b98af81d361c59a53e36ce6e261f0c6549b9f0d5152da21e&)

after test that the password of flag00 user

