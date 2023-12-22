# writeup level10


After execute ls -la in level10 :

```
level10@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level10 level10   140 Mar  6  2016 .
d--x--x--x  1 root    users     340 Aug 30  2015 ..
-r-x------  1 level10 level10   220 Apr  3  2012 .bash_logout
-r-x------  1 level10 level10  3518 Aug 30  2015 .bashrc
-rwsr-sr-x+ 1 flag10  level10 10817 Mar  5  2016 level10
-r-x------  1 level10 level10   675 Apr  3  2012 .profile
-rw-------  1 flag10  flag10     26 Mar  5  2016 token
```




After running binary ninja to reverse this binary :

![main](https://cdn.discordapp.com/attachments/1015186220227231825/1148990230909689946/image.png)
![main2](https://cdn.discordapp.com/attachments/1015186220227231825/1148990437391077406/image.png)


# Explain the code

The code appears to involve system calls like `connect` and `socket`, suggesting an attempt to read a file and transmit its contents to port 6969.

In this code, the initial step involves checking file access permissions using the access function, which checks the accessibility of the file specified in `argv[1]= var_1060`. If the first argument has the required access permissions, the content of the file is sent to port 6969.

```
08048750      if (access(var_1060, type) != 0)
0804874e      {
0804894d          var_1060 = "You don't have access to %s\n";
08048950          eax_27 = printf(var_1060, eax_5);
08048950      }
```

```
08048763          var_1060 = "Connecting to %s:6969 .. ";
08048766          printf(var_1060, eax_7);
08048770          var_1060 = stdout;
08048773          fflush(var_1060);
08048778          int32_t domain = 0;
08048780          int32_t type_1 = 1;
08048788          var_1060 = 2;
0804878f          int32_t fd = socket(var_1060, type_1, domain);
0804879f          int32_t addr = 0;
080487a5          int32_t var_14_1 = 0;
080487ac          int32_t var_10_1 = 0;
080487b3          int32_t var_c_1 = 0;
080487ba          addr = 2;
080487d0          char* cp;
080487d0          in_addr_t var_14_2 = inet_addr(cp);
080487e3          *(uint16_t*)((char*)addr)[2] = htons(0x1b39);
08048805          char const* const var_1048;
08048805          esp = &var_1048;
0804880d          int32_t var_101c;
0804880d          if (connect(fd, &addr, 0x10) == 0xffffffff)
0804880a          {
0804881c              var_1048 = "Unable to connect to host %s\n";
0804881f              printf(var_1048, var_101c);
08048824              var_1048 = 1;
0804882b              exit(var_1048);
0804882b              /* no return */
0804882b          }
08048830          size_t nbytes = 8;
08048838          char const* const buf = ".*( )*.\n";
08048844          char* var_1018;
08048844          var_1048 = var_1018;
0804884f          if (write(var_1048, buf, nbytes) == 0xffffffff)
0804884c          {
0804885e              var_1048 = "Unable to write banner to host %…";
08048861              printf(var_1048, var_101c);
08048866              var_1048 = 1;
0804886d              exit(var_1048);
0804886d              /* no return */
0804886d          }
```

We can test this by listen to port 6969  and send file 


![send](https://cdn.discordapp.com/attachments/1015186220227231825/1149020868383604806/image.png)

![r](https://cdn.discordapp.com/attachments/1015186220227231825/1149020867939016724/image.png)


# Exploit

