---
title: define test 
published: true
---

# define test
## test.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define DEBUG(fmt, ...) printf("\033[31m [%05d]\033[34m-%s: \033[0m" fmt "\n", __LINE__,__func__, ##__VA_ARGS__)

#define ERR_ACTION(condition,action,value,fmt, ...)\
                        if(condition)\
						{\
							DEBUG(fmt,##__VA_ARGS__);\
                            action value;\
						}\

int define_test(int i)
{
    ERR_ACTION(i==0,return,-1,"%s","error value return -1");

    ERR_ACTION(i==1,goto,err,"%s","goto err");
    
    return 0;

err:
    return 1;
}

int main(int argc, const char * argv[]) 
{
    int ret = 1;

    DEBUG("return:%d\n",define_test(ret));

    ret = 0;
    DEBUG("return:%d\n",define_test(ret));
    
    return 0;
}
```
