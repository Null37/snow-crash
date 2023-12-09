# Writeup level05


that we here 2 files one php code and second is executable file 

![level06_ls.png](https://cdn.discordapp.com/attachments/1164485225875783701/1175099598180733058/image.png?ex=6569ffc8&is=65578ac8&hm=654726dd3567ae6c1f47ab305c9df10639ed7db4816ef7d1b05f8b4267d3a010&)


- `d`: Denotes a directory.
- `r-xr-x---`: Represents the file permissions for the owner, group, and others respectively. In this case:
- `r-x` for the owner (level06): Read and execute permissions.
- `r-x` for the group (level06): Read and execute permissions.
- `---` for others: No permissions.
- `+`: Indicates that additional ACLs or extended attributes are set for this file or directory.

Extended ACLs allow more fine-grained control over file permissions beyond the standard owner, group, and others' permissions. These extended attributes might include specific permissions or restrictions set by administrators or programs on a file or directory.

So we can  execute `getfacl level06` to see what what the additional permision the file has:

```
level06@SnowCrash:~$ getfacl  level06
# file: level06
# owner: flag06
# group: level06
# flags: s--
user::rwx
group::---
group:level06:r-x
mask::r-x
other::---
```
- `user::rwx`: The owner (flag06) has read, write, and execute permissions (rwx).
- `group::---`: The owning group has no permissions (---).
- `group:level06:r-x`: The level06 group has read and execute permissions (r-x).
- `mask::r-x`: The mask defines the maximum permissions allowed by the ACL entries. In this case, it's set to read and execute (r-x).
- `other::---`: Others have no permissions (---).

so because the mask we can write in to the file.

source:
- [https://www.redhat.com/sysadmin/linux-access-control-lists](https://www.redhat.com/sysadmin/linux-access-control-lists)
- [https://linux-training.be/storage/ch03.html#:~:text=The%20acl%20mask%20defines%20the,the%20%2D%2Dno%2Dmask%20switch](https://linux-training.be/storage/ch03.html#:~:text=The%20acl%20mask%20defines%20the,the%20%2D%2Dno%2Dmask%20switch)

After saw binaryninija result and execute level06 we can see that level06 execute with php the level06.php and that mean to exploit level06 we need to exploit level06.php

Let explain level06.php:

#!/usr/bin/php
<?php
function y($m) 
{ 
    $m = preg_replace("/\./", " x ", $m); 
    $m = preg_replace("/@/", " y", $m); 
    return $m; 
}
function x($y, $z) 
{
     $a = file_get_contents($y); 
     $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); 
     $a = preg_replace("/\[/", "(", $a); 
     $a = preg_replace("/\]/", ")", $a); 
     return $a; 
}
$r = x($argv[1], $argv[2]); 
print $r;
?>

first x($argv[1], $argv[2]) secript take  from command-line argument argv[1] and argv[2] to fucntion x that function get file content from first argument(argv[1])
and return cotent of this file to a(variable) after that $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);

## preg_replace
The `preg_replace` function, as described in the [PHP documentation](https://www.php.net/manual/en/function.preg-replace.php) Takes the first argument as the pattern, replaces occurrences of this pattern in the subject string (which is taken from the third argument), and substitutes them with the content provided in the second argument.

# **Regular expression:**

Regex, short for regular expression, is a pattern-matching language used to search, manipulate, and analyze text based on specific patterns or rules. It allows you to define patterns that match sequences of characters within a string, making it a powerful tool for tasks like searching for specific words, validating input formats, or extracting information from text

source to learn regex:

- https://www.youtube.com/watch?v=bgBWp9EIlMM&t=130s
- https://www.youtube.com/watch?v=528Jc3q86F8&t=542s

The best websites to visualize regex :

- https://regex101.com/
- [https://www.debuggex.com](https://www.debuggex.com/)



# START EXPLOIT

In our case, the first argument represents the regular expression (regex) pattern. In PHP, regex patterns are typically enclosed between forward slashes (/regex_pattern/). For instance, `(\[x (.*)\])` is a regex pattern that will be checked against the content provided in the third argument

In the line `preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);`, the regex pattern `/(\[x (.*)\])/e `is used. In regular expressions, parentheses () denote groups, so `\[x ] ` represents group 1, and (.*) represents group 2, which captures any string ending in ].
With preg_replace, anything matched by this regex is treated as PHP code due to the /e modifier. Therefore, group 2, captured by (.*), is passed as an argument to the function y using the expression "`y(\"\\2\")".` This means that the content matched by group 2 is passed as an argument to the function y.

you can see in images bellow:

![regex](https://cdn.discordapp.com/attachments/1164485225875783701/1179432986190561360/image.png?ex=6582fe10&is=65708910&hm=90d8ac00f95b160e49ec02d5a7df26a9c66dbbad59f36897156e9b99f9ca1672&)

![regex2](https://cdn.discordapp.com/attachments/1164485225875783701/1181685971104833576/image.png?ex=6581f5d2&is=656f80d2&hm=69e539eaa2bca45301072b2263434929ed20be51926c08247affe4c80191e36a&)

some source:

- https://www.tutorialsteacher.com/regex/grouping

as we know now The /e modifier in preg_replace treats the replacement string as PHP code and evaluates it. so we can use ${} to run our injection to php code

we know In PHP, `${}` allows accessing variables, but it can also be exploited for code injection, such as executing system commands with `${system("ls")}`,  however  we use different method becouse in php code we have argument z that not used in code, even if it's not directly used in the code but can be controlled and used to execute commands.
For exploitation, a file named /tmp/ok is created containing the string [x {${system($z)}}]. By enclosing the payload with {} to avoid potential errors and utilizing the variable $z, we aim to execute system commands through the exploit.

The exploitation is attempted using a command similar to the following:

./level06 /tmp/ok "ls"

![EXPLOIT](https://cdn.discordapp.com/attachments/1164485225875783701/1183034385281335406/image.png?ex=6586dda1&is=657468a1&hm=565206d27b9c22ed17580217954789b5db11c5bd22ba29524d9c8db69b0f570c&)


And that work so we can now replace ls with getflag and get that flag:
