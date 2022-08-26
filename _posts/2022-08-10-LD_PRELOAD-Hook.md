---
title: LD_PRELOAD Hook 
published: true
---

# hook system call

## hook.c
```c
#include <stdio.h>
#include <string.h>
#include <dlfcn.h>

ssize_t write(int fd, const void *buf, size_t count)
{
    static ssize_t (*real_write)(int fd, const void *buf, size_t count) = NULL;

    if (!real_write)
        real_write = dlsym(RTLD_NEXT, "write");

    //do something
    printf("enter write hook\n");

    return real_write(fd,buf,count);
}
```

## test.c
```c
#include <stdio.h>

int main()
{
    write(1,"hello world\n",12);
    return 0;
}
```

## Makefile
```
default:
	gcc -fPIC -shared hook.c -ldl -D_GNU_SOURCE -o libhook.so
	gcc test.c -o test
```

## run
```c
LD_PRELOAD=./libhook.so ./test
enter write hook
hello world
```
