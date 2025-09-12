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
# Project: Embedded Systems Build Makefile
#
# Use:
#   make build [PLATFORM=HOST/MSP432]
#   make clean
#
# Build Targets:
#   build     -> Compiles and links all source files
#   clean     -> Removes object files and binaries
#
# Platform Overrides:
#   PLATFORM=HOST    -> Build and run natively on your PC using GCC
#   PLATFORM=MSP432  -> Cross-compile for MSP432 using arm-none-eabi-gcc
#------------------------------------------------------------------------------

# Makefile for C1M2 Assignment
# Supports HOST (gcc) and MSP432 (arm-none-eabi-gcc)

#------------------------------------------------------------------------------

include sources.mk

# Default platform
PLATFORM ?= HOST

# Output files
TARGET = c1m2.out
MAP = c1m2.map
OBJS = $(SOURCES:.c=.o)

# Tools (default to host, override for MSP432 below)
CC = gcc
LD = gcc
SIZE = size

# General compiler flags
CFLAGS = -Wall -Werror -g -O0 -std=c99
CPPFLAGS = -Iinclude/common
LDFLAGS =

# Platform-specific settings
ifeq ($(PLATFORM),HOST)
    CPPFLAGS += -DHOST
endif

ifeq ($(PLATFORM),MSP432)
    CC = arm-none-eabi-gcc
    LD = arm-none-eabi-gcc
    SIZE = arm-none-eabi-size

    CPPFLAGS += -DMSP432 -Iinclude/msp432 -Iinclude/CMSIS
    LDFLAGS += -T msp432p401r.lds
    CFLAGS += -mcpu=cortex-m4 -mthumb -march=armv7e-m \
              -mfloat-abi=hard -mfpu=fpv4-sp-d16 \
              --specs=nosys.specs
endif

# ----------------------
# Build Rules
# ----------------------

# Default rule
all: build

# Build target (compile + link)
build: $(TARGET)
	$(SIZE) $(TARGET)

# Link final executable
$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) $(CPPFLAGS) $(OBJS) -o $@ -Wl,-Map=$(MAP) $(LDFLAGS)

# Compile source into object
%.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o $@

# Preprocess only
%.i: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -E $< -o $@

# Assembly only
%.asm: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -S $< -o $@

# Compile all objects without linking
.PHONY: compile-all
compile-all: $(OBJS)

# Clean build artifacts
.PHONY: clean
clean:
	rm -f src/*.o src/*.i src/*.asm *.out *.map


