---
title: Traverse Dir  
published: true
---

# Traverse Dir
## test.c
```c
#include <stdio.h>
#include <sys/types.h>
#include <dirent.h>

void traverse_dir(const char* path)
{
    DIR *dirptr=NULL;
    struct dirent *entry;

    if((dirptr = opendir(path))!=NULL)
    {
        while(entry=readdir(dirptr))
        {
            if(strncmp(entry->d_name,".",1) == 0)
                continue;

            printf("%s\n",entry->d_name);
        }
        closedir(dirptr);
    }

    return ;
}

int main()
{
    traverse_dir("./");
    return 0;
}
```
