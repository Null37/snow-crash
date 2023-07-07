exploit binnary level03 with suid 

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

getegid()                                                                                                                                        = 1000
geteuid()                                                                                                                                        = 1000
setresgid(1000, 1000, 1000, 1)                                                                                                                   = 0
setresuid(1000, 1000, 1000, 1)                                                                                                                   = 0
system("/usr/bin/env echo Exploit me"Exploit me
 <no return ...>
--- SIGCHLD (Child exited) ---



Check flag.Here is your token : qi0maab88jeaj46qoumi7maus


