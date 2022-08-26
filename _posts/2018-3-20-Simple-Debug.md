---
title: Simple Debug 
published: true
---

# Simple Debug

## debug.h
```c
#ifndef __DEBUG_H__
#define __DEBUG_H__
#include <time.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/time.h>

FILE* fDebug = fopen("./debug.log", "w+");
char current[100] = "";
long int lTime = 0;
struct tm NowTime;
struct timeval st,et;

#define DebugInfo(fmt,args...)\
        lTime = time(NULL); \
        localtime_r(&lTime, &NowTime);\
        sprintf(current, "[%02d/%02d %02d:%02d:%02d]", NowTime.tm_mon+1, NowTime.tm_mday, \
        NowTime.tm_hour, NowTime.tm_min, NowTime.tm_sec);\
        printf("\033[33m%s %s %d -- " fmt "\033[0m\n", current,  __func__, __LINE__, ##args);

#define PRINT(fmt,args...)\
        printf("\033[33m" fmt "\033[0m\n", ##args);

#define DebugErr(fmt,args...)\
        lTime = time(NULL); \
        localtime_r(&lTime, &NowTime);\
        sprintf(current, "[%02d/%02d %02d:%02d:%02d]", NowTime.tm_mon+1, NowTime.tm_mday, \
        NowTime.tm_hour, NowTime.tm_min, NowTime.tm_sec);\
        printf("\033[31m%s %s %d -- " fmt "\033[0m\n", current, __func__, __LINE__, ##args);

#define WriteDebug(fmt,args...)\
        lTime = time(NULL); \
        localtime_r(&lTime, &NowTime);\
        sprintf(current, "[%02d/%02d %02d:%02d:%02d]", NowTime.tm_mon+1, NowTime.tm_mday, \
        NowTime.tm_hour, NowTime.tm_min, NowTime.tm_sec);\
        fprintf(fDebug,"\033[31m%s %s %d -- " fmt "\033[0m\n", current, __func__, __LINE__, ##args);\
        fflush(fDebug);

#endif
```
