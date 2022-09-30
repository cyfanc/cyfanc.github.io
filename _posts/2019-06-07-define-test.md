---
title: define test 
published: true
---

# define test
## test.c
```c
#include <stdio.h>

#define PRINTF_GREEN(fmt, args...) printf("\033[32m[%s:%s:%d]:["  fmt "]\033[0m\n", __FILE__,__func__,__LINE__,##args);
#define PRINTF_RED(fmt, args...) printf("\033[31m[%s:%s:%d]:[" fmt "]\033[0m\n",  __FILE__,__func__,__LINE__,##args);
#define PRINTF_PINK(fmt, args...) printf("\033[35m[%s:%s:%d]:[" fmt "]\033[0m\n",  __FILE__,__func__,__LINE__,##args);
#define LOG_INFO(fmt,args...) PRINTF_GREEN(fmt,##args)
#define LOG_DEBUG(fmt,args...) PRINTF_PINK(fmt,##args)
#define LOG_ERR(fmt,args...) PRINTF_RED(fmt,##args)
#define LOG(level,fmt,args...) LOG_##level(fmt,##args)
#define DEBUG(fmt, ...) LOG_DEBUG(fmt,##__VA_ARGS__)
#define ERR_ACTION(condition, action, value, fmt, ...) \
    if (condition)                                     \
    {                                                  \
        LOG_INFO(fmt, ##__VA_ARGS__);                  \
        action value;                                  \
    }

int main()
{
    LOG(ERR,"%s","111");
    LOG(INFO,"%s","222");
    LOG(DEBUG,"%s","333");
    ERR_ACTION(1, return, -1, "%s", "error value return -1");
    
    return 0;
}
```
