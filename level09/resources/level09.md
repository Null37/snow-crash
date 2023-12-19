# writeup level09


At this level, we have a file token with read accesses to the token and suid level09 can execute as level09:

```
level09@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level09 level09  140 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level09 level09  220 Apr  3  2012 .bash_logout
-r-x------ 1 level09 level09 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag09  level09 7640 Mar  5  2016 level09
-r-x------ 1 level09 level09  675 Apr  3  2012 .profile
----r--r-- 1 flag09  level09   26 Mar  5  2016 token
```

And the token has non-printable character so is obfuscated  token

```
level09@SnowCrash:~$ cat token
f4kmm6p|=�p�n��DB�Du{��
```


After testing the executable ./level09 we can see that level09 is the script that obfuscates token:

```
level09@SnowCrash:~$ ./level09 aaa
abc
level09@SnowCrash:~$ ./level09 aa
ab
level09@SnowCrash:~$ ./level09 123
135
level09@SnowCrash:~$ ./level09 987
999
level09@SnowCrash:~$ ./level09 997
9:9
level09@SnowCrash:~$ 
```

So, we can understand that the encoding process simply adds a number to each ascii character based on its position in the line of characters, starting from 0 for the first character. for example:

aaa : 

- a (position  0) + 0 = a
- a(position 1) + 1 = b
- a (position  2) + 2 = c


# decode 

For that theory, we can write code to decode it:


```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(int argc, char** argv) {
    FILE *file;
    char line[100];

 
    file = fopen(argv[1], "r");

   fgets(line, 200, file);
 
    int i = 0;
   for(; line[i] != '\0'; i++)
   {
        line[i] = line[i] - i;
   }
   line[i] = '\0';
   printf("%s", line);
fclose(file);

    return 0;
} 
```

after test to token we can get  token : f3iji1ju5yuevaus41q1afiuq� and one non-printable character.
so after test to flag09 we can get the flag 