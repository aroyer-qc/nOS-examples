# Binaries will be generated with this name (.elf, .bin, .hex, etc)
EXEC=GCC_STM32F429I-Discovery

# Put your source files here (or *.c, etc)
ASMS:=$(wildcard src/*.S)
SRCS:=$(wildcard src/*.c) $(wildcard SPL/src/*.c) $(wildcard nOS/src/*.c) $(wildcard nOS/src/port/GCC/ARM_Cortex_M4/*.c)
INCS=-Iinc -Icmsis -Iconfig -ISPL/inc -InOS/inc -InOS/inc/port/GCC/ARM_Cortex_M4
LIBS=-L/usr/arm-none-eabi/lib/armv7-m -lc -L/usr/lib/gcc/arm-none-eabi/4.9.3/armv7-m -lgcc

# Normally you shouldn't need to change anything below this line!
#######################################################################################

PREFIX=arm-none-eabi-
CC=$(PREFIX)gcc
AS=$(PREFIX)as
LD=$(PREFIX)ld
OBJCOPY=$(PREFIX)objcopy

CFLAGS   = -g -O2 -Wall
CFLAGS  += -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork
CFLAGS  += -mfloat-abi=hard -mfpu=fpv4-sp-d16
AFLAGS   = -g -mlittle-endian -mthumb -mcpu=cortex-m4 -mthumb-interwork
AFLAGS  += -mfloat-abi=hard -mfpu=fpv4-sp-d16
LDFLAGS  = -Tstm32f429zi_flash.ld -Map $(EXEC).map -nostartfiles

OBJS     = $(ASMS:.S=.o) $(SRCS:.c=.o)

.PHONY: all clean

all: $(EXEC).elf

.S.o:
	@echo Assembling $<
	@$(AS) $(AFLAGS) -g $< -o $@

.c.o:
	@echo Compiling $<
	@$(CC) $(CFLAGS) $(INCS) -c $< -o $@

$(EXEC).elf: $(OBJS)
	@echo Linking $@
	@$(LD) $(LDFLAGS) $(OBJS) -o $@ $(LIBS)
	@$(OBJCOPY) -O ihex $(EXEC).elf $(EXEC).hex
	@$(OBJCOPY) -O binary $(EXEC).elf $(EXEC).bin

clean:
	-@rm -f $(OBJS) $(EXEC).elf $(EXEC).map $(EXEC).hex $(EXEC).bin 2>/dev/null || true
