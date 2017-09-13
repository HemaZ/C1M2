#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
PLATFORM = HOST


OBJS := $(SOURCES:.c=.o)


# LD = 
# LDFLAGS = 
# CFLAGS = 
# CPPFLAGs = 




	ifeq ($(PLATFORM),HOST)
		CC = gcc
		CFLAGS = -Wall -g -O0 -std=c99 -DHOST

	else
		CC = arm-none-eabi-gcc
		CFLAGS = -DMSP432 $(COMMON_CFLAGS)
        LINKER_FILE = ../msp432p401r.lds
		LDFLAGS = -T $(LINKER_FILE)
		CPU = cortex-m4
		SPECS=nosys.specs
		ARCH = \
	     -mcpu=$(CPU) \
	     -mthumb \
	     -march=armv7e-m \
	     -mfloat-abi=hard \
	     -mfpu=fpv4-sp-d16 \
	     --specs=$(SPECS)

	endif

%.i : %.c
	$(CC) -E $(SOURCES) $(CFLAGS) $(INCLUDES) -C
	
%.asm: %.c
	$(CC) -S $(SOURCES) $(CFLAGS) $(INCLUDES) -C 

%.o: %.c
	$(CC) -c $< $(CFLAGS) -o $@ $(LDFLAGS) $(INCLUDES) -MD

.PHONY:compile-all 
compile-all: $(OBJS)
	gcc -c $(CFLAGS) $(OBJS)

.PHONY:build
build: $(OBJS)
	$(CC) $(CFLAGS) $(ARCH) $(INCLUDES) $(LDFLAGS) -Xlinker -Map=c1m2.map -o c1m2.out $(OBJS)
	size c1m2.out

.PHONY:clean
clean:
	rm -f *.map *.out *.o *.asm *.i *.s	*.d