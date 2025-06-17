# Bootloader

> **Random Quote:** Every operating system begins with 512 bytes of faith.

A bootloader is a program stored in the boot sector (usually the first 512 bytes of a bootable storage device) that the BIOS loads into memory and executes.

## Boot Sector and Boot Signature
Learn:
+ What is the boot sector.
+ What is the boot signature and why is it useful.


## Memory Layout in real mode

+ 1 MiB addressable space.
+ Conventional memory, upper memory, BIOS, etc.
+ The bootloader's size and location.

## Memory Addressing

+ Segmentation and Offset.
+ Memory regions (`0x0000` - `0xFFFF`)

## Calculating Memory Sizes

+ Calculating size of memory ranges (e.g. `0x7C00` to `0x7CFF`)
+ Understanding kilobytes, megabytes, etc, in binary form (1 KiB = 1024 bytes).

## Memory-Mapped I/O (MMIO)

+ Why devices appear as memory addresses.
+ Why `0xB8000` maps to video memory in text mode.
+ Early interaction with screen/keyboard via memory.

## Reading the kernel
+ Revisit BIOS interrupt (`INT 0x13`) to read the OS kernel from disk into memory.
+ You don't need to learn about filesystems now, but you can if you want to.

## A20 Line
+ What is the A20 line?
+ Why do we need to enable the A20 line?
+ Different ways of enabling the A20 line. Learn about all the different ways but use the most practical/efficient one.

## GDT (Global Descriptor Table)
Learn:
+ What the GDT is.
+ Its role in switching to protected mode.

## Switching to Protected Mode
+ Disabling interrupts
+ Loading the GDT
+ Setting the PE bit in the `CR0` register.
+ Far jump to 32-bit code.

## Bootloader Stages (optional)
Learn:
+ About first stage and second stage bootloaders.
+ Why they exist.
+ Their different roles.
