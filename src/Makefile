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
		LINKER_FILE = msp432p401r.lds
		CPU = cortex-m4
		ARCH = thumb
		SPECS = nosys.specs
		LDFLAGS = -Wl,-Map=c1m2.map -T $(LINKER_FILE)
		CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS) -Wall -g -O0 -std=c99 -DMSP432

	endif

%.i : %.c
	$(CC) -E $(SOURCES) $(CFLAGS) $(INCLUDES) -C
	
%.asm: %.c
	$(CC) -S $(SOURCES) $(CFLAGS) $(INCLUDES) -C

%.o: %.c
	$(CC) -c $< $(CFLAGS) -o $@ $(LDFLAGS) $(INCLUDES)

.PHONY:compile-all 
compile-all: $(OBJS)
	gcc -c $(CFLAGS) $(OBJS)

.PHONY:build
build: $(OBJS)
	$(CC) –O c1m2.out $(SOURCES) $(LDFLAGS) $(INCLUDES) $(CFLAGS)

.PHONY:clean
clean:
	rm -f *.map *.out *.o *.asm *.i *.s	