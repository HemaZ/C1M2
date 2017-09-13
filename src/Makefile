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
# <MAKE File FOR HOST and MSP432>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <FILE>.i Build preprocessed file
#      <FILE>.asm Build assembly file
#      <FILE>.o Build object file 
#      compile-all Compile all object files
#      build Compile and link project.out
#      clean Remove all generated files
#
# 
#------------------------------------------------------------------------------
include sources.mk

OBJS := $(SOURCES:.c=.o)


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
	$(CC) $(CFLAGS) $(ARCH) $(INCLUDES) -MD -c $< -o $@

.PHONY:compile-all 
compile-all: $(OBJS)
	gcc -c $(CFLAGS) $(OBJS)

.PHONY:build
build: $(OBJS)
	$(CC) $(CFLAGS) $(ARCH) $(INCLUDES) $(LDFLAGS) -Xlinker -Map=c1m2.map -o c1m2.out $(OBJS)
	size c1m2.out

.PHONY:clean
clean:
	rm -f *.d *.map *.out *.o *.asm *.i *.s	