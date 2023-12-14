# writeup level08

In level08 we have again level08 setuid file 

```
level08@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout
-r-x------  1 level08 level08 3518 Aug 30  2015 .bashrc
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-r-x------  1 level08 level08  675 Apr  3  2012 .profile
-rw-------  1 flag08  flag08    26 Mar  5  2016 token
level08@SnowCrash:~$ 

```




![level08](https://cdn.discordapp.com/attachments/1015186220227231825/1126841126897332244/image.png)



# Explain

we have here :
```
080485c1      if (strstr(argv[1], "token") != 0)
080485bf      {
080485d8          printf("You may not access '%s'\n", argv[1]);
080485e4          exit(1);
080485e4          /* no return */
080485e4      }

```
first we have strstr checks if "token" is present within argv[1] and if true printf print error message and programe exit
like that : 
```
level08@SnowCrash:~$ ./level08 /tmp/token
You may not access '/tmp/token'
```
```
level08@SnowCrash:~$ ./level08 token
You may not access 'token'
```

And then  programe open the file using  int id = open(argv[1], 0); with passing 0 as the second argument that means no specific flags or options are being set.
And then progrgame write what in argv[1] to 1(stdout) in terminal.

# Exploit

The open() function in C follows symbolic links if the path names a symbolic link, allowing us to create a symbolic link to 'token' with a different name.

### source 

- [open](https://linux.die.net/man/3/open)
- [soft link](https://www.redhat.com/sysadmin/soft-links-linux)

So we can just create symblic link:

```
ln -s $PWD/token /tmp/tt
```
After some test the token: quif5eloekouj29ke0vouxean that password for falg08


