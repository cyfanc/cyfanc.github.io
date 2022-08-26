---
title: Timestamp To Date  
published: true
---

# Timestamp To Date
## test.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

char *timestamp2date(time_t time)
{
    struct tm tmDate;

    char *p = (char *)malloc(20);

    localtime_r(&time, &tmDate);

    strftime(p, 20, "%Y-%m-%d %H:%M:%S", &tmDate);

    return p;
}

time_t date2timestamp(char *time)
{
    struct tm stm;

    strptime(time, "%Y-%m-%d %H:%M:%S", &stm);

    return mktime(&stm);
}

int main()
{
    char* date = timestamp2date(time(NULL));

    int timestamp = date2timestamp(date);

    printf("date:%s\n",date);

    printf("timestamp:%d\n",timestamp);

    free(date);
}
```
