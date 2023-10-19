# Writeup level01

From the previous level00 enumeration, we found in /etc/passwd the hashed password for user flag01: 42hDRfypTqqnw



password /etc/passwd format source : https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/

### why password save in /etc/password

Historically, in the old days, the hash of the password was saved in /etc/passwd, and now the hashed password is saved in /etc/shadow.
source: https://unix.stackexchange.com/questions/542989/when-did-unix-stop-storing-passwords-in-clear-text

## crack the hash
We use john the reaper to crack the hash, the password is : abcdefg

![cracked](https://cdn.discordapp.com/attachments/1164485225875783701/1164502146172059688/image.png?ex=65437223&is=6530fd23&hm=29d61e485129da1b7d4dd97cef689ae2d02ccbb8f5e417486f10d38b61921c02&)
