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

# Include sources and include paths
include sources.mk

# Default platform (can be overridden via command line)
PLATFORM = HOST

# Output file name
TARGET = final_program

#------------------------------------------------------------------------------
# Compiler/Linker Flags
#------------------------------------------------------------------------------

# Common flags for all builds
CFLAGS = -Wall -Werror -g -O0 -std=c99
CPPFLAGS = $(INCLUDES)
LDFLAGS = 

#------------------------------------------------------------------------------
# Platform Specific Settings
#------------------------------------------------------------------------------
ifeq ($(PLATFORM),HOST)
    CC = gcc
    LD = gcc
    ARCH =
    SPECS =
    CPU =
    LINKER_FILE =
endif

ifeq ($(PLATFORM),MSP432)
    CC = arm-none-eabi-gcc
    LD = arm-none-eabi-ld
    ARCH = -mthumb
    CPU = -mcpu=cortex-m4
    SPECS = --specs=nosys.specs
    LINKER_FILE = ../msp432p401r.lds
    LDFLAGS = -T $(LINKER_FILE)
    CPPFLAGS += -DMSP432
endif

#------------------------------------------------------------------------------
# Build Rules
#------------------------------------------------------------------------------

# Object files from source list
OBJS = $(SOURCES:.c=.o)

# Default target
build: $(TARGET)

# Linking step
$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) $(ARCH) $(CPU) $(OBJS) -o $(TARGET) $(LDFLAGS) $(SPECS)

# Compilation step for each source file
%.o: %.c
	$(CC) $(CFLAGS) $(ARCH) $(CPU) $(CPPFLAGS) -c $< -o $@

# Clean up
clean:
	rm -f $(OBJS) $(TARGET)

