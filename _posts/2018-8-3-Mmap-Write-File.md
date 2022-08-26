---
title: Mmap Write File  
published: true
---

# Mmap Write File
## test.c
```c
#include <stdio.h>  
#include <string.h>
#include <sys/mman.h>
#include <fcntl.h>

#define MMAP_SIZE 10*1024

int main(int argc, char** argv)
{
    int fd;
    char* addr = NULL;

    fd = open("./log", O_RDWR | O_CREAT, 0644);

    if(ftruncate(fd,MMAP_SIZE))
        return -1;

    if((addr=(char*)mmap(NULL,MMAP_SIZE,PROT_WRITE|PROT_READ,MAP_SHARED,fd,0)) == NULL)
		return -1;

    memcpy(addr,"hello world\n",12);
    
    munmap(addr, MMAP_SIZE);

    close(fd);

    return 0;
}
```
