# Writeup level03

we have binnary here with suid permissions 



![level03](https://cdn.discordapp.com/attachments/1164485225875783701/1171884278418768003/image.png?ex=655e4d48&is=654bd848&hm=f9f6830ea102c2663e5f8c3330639e8226cec8345541f55d1489a18e74a3b270&)

first let reverse engineering the code and the exploit binnary level03 with suid


level03 permission:

he permissions -rwsr-sr-x on the file level03 indicate the following:

The first character - indicates that it is a regular file.
The next three characters rws indicate the permissions for the owner (flag03).
- r stands for read permission.
- w stands for write permission.
- s stands for the setuid permission, which means that the file will be executed with the privileges of the file owner (flag03).
The next three characters sr- indicate the permissions for the group (level03).
-s stands for the setgid permission, which means that the file will be executed with the privileges of the group owner (level03).
- r stands for read permission.
- - indicates no write permission.
The last three characters r-x indicate the permissions for others.
- r stands for read permission.
- - indicates no write permission.
- x stands for execute permission.

any more details about SUID/SGID ==> https://www.redhat.com/sysadmin/suid-sgid-sticky-bit

let donwload the level03 file to our machine with scp or open server with python:

scp -P 4242 level03@'<machine ip>':/home/user/level03/levl03 .

if we used ltrace to see functions that are used in  this binary we can see :

getegid()                                                                                                                                        = 1000
geteuid()                                                                                                                                        = 1000
setresgid(1000, 1000, 1000, 1)                                                                                                                   = 0
setresuid(1000, 1000, 1000, 1)                                                                                                                   = 0
system("/usr/bin/env echo Exploit me"Exploit me
 <no return ...>
--- SIGCHLD (Child exited) ---

or use binary ninja to reverse the binary file

![level03 main](https://cdn.discordapp.com/attachments/1164485225875783701/1171920869380083823/image.png?ex=655e6f5c&is=654bfa5c&hm=ab8d20009deab70d889538750872a3dbd1f18d2f8c64bee6f2551dffceb242ba&)


`system("/usr/bin/env echo Exploit me");`


system function used to execute the command, which means that the command in our case executes as flag03.

The vulnerability here lies in the fact that the developer of the level03 binary uses an relative path for the `echo` command. We can manipulate echo because, as we know the shell, we can retrieve the path of echo from the environment variable `PATH` and search for the path from left to right.

We can create a new echo executable in the /tmp directory, and our goal is to obtain the `flag03` flag. To achieve this, we only need to execute `getflag` as the flag03 user

like that :
```
#!/bin/bash

getflag
```
![level03 GG](https://cdn.discordapp.com/attachments/1164485225875783701/1171926734539591700/image.png?ex=655e74d2&is=654bffd2&hm=badfeaaf82f0fe37628bf33d5b8212087907529d7baf17361cfc1f0d822cff24&)




