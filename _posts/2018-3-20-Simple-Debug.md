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

# log.h
```c
#ifndef __LOG_H__
#define __LOG_H__

#include <time.h>
#include <stdio.h>
#include <errno.h>
#include <string.h>
#include <stdarg.h>
#include <stdlib.h>

#define WRITELOG(nLogType,fmt,...) writeLog(nLogType,__FILE__,__func__,__LINE__,fmt,__VA_ARGS__)

#define ERROR 0
#define DEBUG 3
#define WARRING 8

const char* color[] = {
        "\033[31m", //red
        "\033[32m", //green
        "\033[33m", //yellow
        "\033[34m", //blue
        "\033[35m", //pink
        "\033[41m", //background red
        "\033[90m", //grey
        "\033[91m", //light red
        "\033[95m"  //light magenta
};

struct LogVar
{       
        FILE* fp;
        char cTime[50];
        struct tm time;
        long int lTime;
        char cLog[1024];
};

struct LogVar g_log;

void writeLog(int type,const char* file, const char* func,const int line,const char *fmt,...)
{
        //获取参数
        va_list vp;
        va_start(vp,fmt);
        vsnprintf(g_log.cLog,1023,fmt,vp);
        va_end(vp);

        //获取时间
        g_log.lTime = time(NULL);
        localtime_r(&g_log.lTime, &g_log.time);
        sprintf(g_log.cTime, "[%d-%02d-%02d %02d:%02d:%02d]", g_log.time.tm_year+1900,g_log.time.tm_mon+1, g_log.time.tm_mday, \
        g_log.time.tm_hour, g_log.time.tm_min, g_log.time.tm_sec);

        switch (type)
        {
                case ERROR:
                        memcpy(g_log.cTime + strlen(g_log.cTime),"[ERROR]",9);
                        break;
                case WARRING:
                        memcpy(g_log.cTime + strlen(g_log.cTime),"[WARRING]",9);
                        break;
                default:
                        memcpy(g_log.cTime + strlen(g_log.cTime),"[INFO]",9);
                        break;
        }

        printf("%s%s [%s][%s][%d] [%s]\033[0m\n",color[type],g_log.cTime,file,func,line,g_log.cLog);

        return ;
}

#endif
```
