---
title:  Makefile Demo  
published: true
---

# Makefile Demo
## Makefile template
```
#COMPLILER USED
CC = gcc
AR = ar cru

#INCLUDE
SRC_PATH=./

INCLUDE_PATH=./

LIB_PATH=./

#BIN_DIR
BIN_DIR=./bin

#OBJS          
OBJS=$(patsubst %.c, %.o, $(wildcard $(SRC_PATH)/*.c))

#SO FLAG
SHARE = -shared -fPIC -rdynamic -ldl

#COMPILE FLAGS
ADD_INCL = $(addprefix -I, $(INCLUDE_PATH))
ADD_LIB = $(addprefix -L, $(LIB_PATH))

#LAST FLAG
CFLAGS = -Wall -g -DLINUX  -ldl
CFLAGS  += $(ADD_INCL)
LDFLAGS += $(ADD_LIB)

#OUTPUT
PROGRAME=test

all: ${PROGRAME}

#MAKE RULE
$(PROGRAME):$(OBJS)
    $(CC) -o $@ $^ $(CFLAGS) $(LDFLAGS)

%.o: %.c
    $(CC) -c -o $@ $< $(CFLAGS) $(LDFLAGS)

clean:
    $(RM) $(PROGRAME) $(OBJS)

install:
ifneq ($(strip $(PROGRAME)),)
    mkdir -p $(BIN_DIR)
    mv $(PROGRAME) $(BIN_DIR)
endif
```

## find path make
```
#filter path
FILTER_PATH :=. 

#get makefile
DIR=$(shell find ./ -maxdepth 4 -name makefile)

#get makefile path
SUBDIR := $(shell dirname $(DIR))

#filter
SUBDIR := $(filter-out $(FILTER_PATH), $(SUBDIR))

default:
ifneq ($(strip $(SUBDIR)),)
    @echo
    @$(foreach sd, $(SUBDIR), $(MAKE) -C $(sd) &&) echo
    @echo
endif

clean:
ifneq ($(strip $(SUBDIR)),)
    @echo
    @$(foreach sd, $(SUBDIR), $(MAKE) clean -C $(sd) &&) echo
    @echo
endif

install:
ifneq ($(strip $(SUBDIR)),)
    @echo
    @$(foreach sd, $(SUBDIR), $(MAKE) install -C $(sd) &&) echo
    @echo
endif
```

## cross-compilation openwrt
```
#env
ENV=../openwrt-sdk-18.06.1-ramips-mt76x8_gcc-7.3.0_musl.Linux-x86_64/staging_dir/toolchain-mipsel_24kc_gcc-7.3.0_musl
ENV_INCLUDE=$(ENV)/include
ENV_LIB=$(ENV)/lib

#COMPLILER USED
CC =$(ENV)/bin/mipsel-openwrt-linux-musl-gcc-7.3.0

#VARIABLR
INCLUDE=./
LIB=./
 
#COMPILE FLAGS
ADD_INCL = -I$(INCLUDE) -I$(ENV_INCLUDE)
ADD_LIB = -L$(LIB) -L$(ENV_LIB)

#LAST FLAG
CFLAGS = -Wall -g -DLINUX  -funsigned-char
CFLAGS  += $(ADD_INCL)
LDFLAGS += $(ADD_LIB)

SRC_PATH=./src

OBJ=$(patsubst %.c, %.o, $(wildcard $(SRC_PATH)/*.c))

OUTPUT=capture

BIN_DIR=./bin
 
#MAKE RULE
$(OUTPUT):$(OBJ)
    $(CC) -o $@ $<  $(CFLAGS) $(LDFLAGS)

%.o:%.c
    $(CC) -c -o $@ $<  $(CFLAGS) $(LDFLAGS)

clean:
    $(RM) $(OBJ) $(OUTPUT)

install:
ifneq ($(strip $(OUTPUT)),)
    mv  $(OUTPUT) $(BIN_DIR)
endif
```
