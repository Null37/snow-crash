![BinaryNinja](https://cdn.discordapp.com/attachments/1015186220227231825/1129027521418100816/image.png)

putchar 





![BinaryNinja](https://cdn.discordapp.com/attachments/691741851648262195/1129130630945443922/image.png)


```
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

#define MAX_LINE_LENGTH 200

int main(int argc, char** argv) {
    FILE *file;
    char line[MAX_LINE_LENGTH];

 
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

token : f3iji1ju5yuevaus41q1afiuq




flag 09: s5cAJpM8ev6XHw998pRWG728z
