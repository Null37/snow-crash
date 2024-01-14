# writeup level11


In this level, we also have a setuid file with the extension lua (Lua programming language):

![level11](https://cdn.discordapp.com/attachments/1164485225875783701/1195286630806859846/image.png?ex=65b3706c&is=65a0fb6c&hm=8fca8d4cb5e34438822fce938bc108f37e8c32c857f53eafd65f2bfb694d2349&)


In  level11.lua file :

```
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end


while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

We attempted to open port `5151`, but it was already bound. This indicates that the file is already running the program. This is the intended behavior of the program. To confirm, we used `telnet` to establish a connection

```
telnet localhost 5151
```

![telnet](https://cdn.discordapp.com/attachments/1164485225875783701/1196089519628361850/image.png?ex=65b65c2b&is=65a3e72b&hm=fe5d48f5629ac26ca9e301a8421143b0b284c56181dbce95207d9fe04170ee98&)

So, we are certain that the file is running the same program in the background. Additionally, we have the hash `f05d1d066fb246efe0c6f7d095f909a7a0cf34a0`. After spending some time attempting to crack it with John and Hashcat, we resorted to online hash cracking at `https://crackstation.net/`.

![crack hash](https://cdn.discordapp.com/attachments/1164485225875783701/1196090883645374514/image.png?ex=65b65d70&is=65a3e870&hm=dcbbb88298450cf774e1ad07be02c61d9f100387dcd50a61cf1d2db6b2187295&)

we try that password in flag11 and level11 and nothing work.


# Exploit 

So, returning to the code, in the line `prog = io.popen("echo "..pass.." | sha1sum", "r")`, we can observe that the code utilizes a command line instead of a function to obtain the hash. This creates a vulnerability known as command-line injection, as there is no input filtering for user input.

the exploit is :

![explot](https://cdn.discordapp.com/attachments/1164485225875783701/1196093512807100466/image.png?ex=65b65fe3&is=65a3eae3&hm=bd3e5657aafe0a8a403b4172bb9c566682a163bc67dbbb795ad955fff11ad4b2&)

we can simply execute `getflag` and redirect the output to a file since we cannot view the output directly.