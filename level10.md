# LEVEL1O


After execute ls -la in level10 :

![ls -la](https://cdn.discordapp.com/attachments/1015186220227231825/1148206472694616124/image.png)


after run binary ninja to reverse the elf we got:

![main](https://cdn.discordapp.com/attachments/1015186220227231825/1148990230909689946/image.png)
![main2](https://cdn.discordapp.com/attachments/1015186220227231825/1148990437391077406/image.png)


after reading the code we can understand that the elf file read a file that the user "level10" has access to and sent it to port 6969 as you see below: 


![send](https://cdn.discordapp.com/attachments/1015186220227231825/1149020868383604806/image.png)

![r](https://cdn.discordapp.com/attachments/1015186220227231825/1149020868383604806/image.png)

in code we have an condition : 
08048746      var_1060 = argv[1];
08048750      int32_t eax_29;
08048750      if (access(var_1060, 4) != 0)
0804874e      {
0804894d          var_1060 = "You don't have access to %s\n";
08048950          eax_29 = printf("You don't have access to %s\n", eax_5);
08048950      }
