# writeup level12

In this level, there is also a setuid executable:

![ls](https://cdn.discordapp.com/attachments/1164485225875783701/1196428251728453632/image.png?ex=65b797a3&is=65a522a3&hm=2b1a6fb8b339da662b6631300c6a11e8311cb1066830f5572aabc2bca5f7d6b3&)

After execute level13

```
level13@SnowCrash:~$ ./level13
UID 2013 started us but we we expect 4242

```
So, the binary gives you something if you match the UID 4242; however, after reverse engineering the binary, we discovered:


![main 1](https://cdn.discordapp.com/attachments/1164485225875783701/1196430410561880094/image.png?ex=65b799a6&is=65a524a6&hm=ab9847cc7be735ac3efe6f655037e186bbd08f6ed1befacf7d4d0782a4225955&)

# Exploit

The `0x1092` is in hexadecimal and is equal to 4242. In this binary, we can be sure that if the UID is 4242, it gives you a token. The token you receive is obtained after the function 'ft_des(char* arg1)' decrypts the string `boe]!ai0FB@.:|L6l@A?>qJ}`.

So, we can unravel the ft_des puzzle, understand the function, and reverse engineer the binary. Alternatively, we can exploit the binary and change our UID from 2013 to 4242:


![assembly intel syntax](https://cdn.discordapp.com/attachments/1164485225875783701/1196442421475676230/image.png?ex=65b7a4d6&is=65a52fd6&hm=284245a964c42c16c9320d697356cfc4b7431237891a507f10afb23040e7b624&)


Now, we can jump to change the return value of `getuid`. To do that, we can navigate to the address in `0x08048595` where the call to `0x8048380 <getuid@plt> `occurs and edit `eax`, which represents the return value in 32-bit assembly. To achieve this, we set a breakpoint at `0x08048595` using break *address in GDB. Then, we can edit the eax value within GDB using the command `set $register = value.` For example, `set $eax = 0x1092`. Finally, we run the program

![register](https://cdn.discordapp.com/attachments/1164485225875783701/1196445370880893038/image.png?ex=65b7a795&is=65a53295&hm=c42d21ba23678eb155879b711a57dcfe3ee7477c58ba8dbb9db24e5cb78aed32&)

Just `next` and we can take the token:

![token](https://cdn.discordapp.com/attachments/1164485225875783701/1196445834716385310/image.png?ex=65b7a803&is=65a53303&hm=18ffd99268f803f307d825b60e6b9bdfd8827280e970dcf6e28693270400c999&)
