# writeup level14

After spending some hours at this level, we couldn't find anything. Therefore, we decided to exploit the `getflag` binary as the only solution we could think of.

So we download the getflag `scp -P 4242 level14@192.168.1.3:/bin/getflag .`

![getflag](https://cdn.discordapp.com/attachments/1164485225875783701/1196561374428549211/image.png?ex=65b8139e&is=65a59e9e&hm=02ee6d7292593f0c78857c73653358cc984304d4fabb6263fdb888012d1d57b4&)

Here we have the some function `ft_des(char* arg1)` from the level13.
After analyzing the code, we can understand that the program checks the current user ID (uid) and issues a token based on specific conditions within if and else statements. In the conditions, we observe that the program checks if the return value of getuid is equal to 0xbbe, 0xbc4, or 0xbc5. This implies that if the user ID matches any of these values, we can verify this information in the /etc/passwd file and compare the uid of the "flagxx" user. The program seems to check if the user ID matches any of these conditions and, if true, provides a token

so 0xbbe=3006, 0xbc4=3012, 0xbc5=3013, and 0xbc6=3014 that use id of flag14

'/etc/passwd':


![etc/passwd](https://cdn.discordapp.com/attachments/1164485225875783701/1196566263267344455/image.png?ex=65b8182c&is=65a5a32c&hm=0c4eed22a64fce61e71b00b49eebacf69ac700501ed736f1b86b1bb6b329cbbc&)


# Exploit

As we did in level 13, we can navigate to the address of the return value of getuid and modify it to `0xbc6` to pretend we are the user `flag14`. To achieve this, we can use GDB (GNU Debugger). However, we face a small challenge with anti-debugging measures. The first anti-debugging check is as follows:

```
if (ptrace(PTRACE_TRACEME, 0, 1, 0, 0) < 0)
{
    puts("You should not reverse this");
    exit(1);
}
```
This code utilizes the ptrace library, commonly used by debuggers such as GDB. It checks if the process is being traced and exits with a message if it detects debugging attempts. To circumvent these challenges, we can use the `ni` shortcut for the next instruction in GDB. By doing so, we can proceed to edit the value in the eax register, which stores the return value of ptrace, ensuring it is greater than 0. Subsequently, we navigate through the code line by line, addressing potential anti-debugging measures like isLib. Finally, we reach the address 0x8048afd, the location of getuid, where we modify the eax return value to 0xbc6, corresponding to the user ID of flag14, allowing us to obtain the flag.

![flag](https://cdn.discordapp.com/attachments/1164485225875783701/1196574199800680478/image.png?ex=65b81f90&is=65a5aa90&hm=9fe5eab2a5f10e75ff43fa9669e805ebd58efa1d39ee98481fe165c72e1cd696&)



