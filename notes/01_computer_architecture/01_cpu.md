# CPU

> **Random Quote:** At the heart of every computer is a tiny control freak, obsessed with order, timing, and doing one thing at a time very, very fast.

The **CPU (Central Processing Unit)** is the part of the computer that runs programs. It reads instructions, figures out what they mean, and carries them out, over and over again, really fast.

**Contents:**
+ [The Fetch, Decode, Execute Cycle](#the-fetch-decode-execute-cycle)
+ [Boot Sequence](#boot-sequence)
+ [Processor Modes](#processor-modes)


# The Fetch, Decode, Execute Cycle
This cycle is also known as the **instruction cycle**. It's the repeating three-step process the CPU uses to read and carry out instructions from memory. It repeates indefinitely until you shutdown the computer.

The cycle has 3 main phases:
+ **Fetch:** Get the next instruction from memory.
+ **Decode:** Understand what the instruction means.
+ **Execute:** Carry out the instruction.

![fetch, decode, execute image](../../resources/images/fetch-decode-execute-cycle.png)

**Fetch:** The **Instruction Pointer (IP / EIP / RIP)** holds the memory address of the next instruction. The CPU fetches the instruction from memory at that address and loads it into the **Instruction Register (IR)**. Then it increments the instruction pointer (unless the instruction says otherwise, like a jump or call).

**Decode:** The *Control Unit* interprets the instruction's operation code. It figures out what operation to perform (e.g, `ADD`, `MOV`, etc), which registers or memory addresses are involved, what kind of data it's working with, among other things.

**Execute:** The CPU activates the appropriate hardware (like the ALU (Arithmetic Logic Unit) for math or logic, memory access units for load/store, control lines for jumps or system-level instructions, etc) and executes the instruction. The result may change registers, flags, or memory.

After execution, the process repeats from the top, fetch. Wanna know something mind-blowing, this cycle can happen **billions of times per second.** What?

> In OS dev, you will eventually write code that sets up the instruction pointer and manages memory, crucial for switching contexts or handling traps.


# Boot Sequence
When you turn on the computer, the CPU doesn't start in your OS, it starts in firmware.
This is the boot sequence: `Power-on >> BIOS/UEFI >> Bootloader >> Kernel`.

+ **Power-on:** When you hit the power button, the motherboard provides power to the CPU and other components. The CPU resets and starts executing instructions from a fixed address. On x86, this is typically `0xFFFFFFF0`, the top of the 32-bit address space.
+ **BIOS/UEFI:** *BIOS (Basic Input/Output System)* or UEFI (modern replacement) is firmware stored on the motherboard. It initializes the hardware (CPU, RAM, keyboard controller, video, etc), then it looks for a boot device (HDD, USB, SDD, etc) and finally loads the bootloader from the boot device to memory. In BIOS systems, the bootloader is usually in the **MBR (Master Boot Record)** (the first 512 bytes of the disk).
+ **Bootloader:** A bootloader (e.g., GRUB) is a small program that loads the OS kernel into memory and transfers control to it. It also switches to protected mode in 32-bit systems or long mode in 64-bit systems before transferring control to the kernel. CPU modes are discussed below.
+ **Kernel:** The kernel (the main part of the OS) does everything else; set up memory management, initialize interrupts, mounts a root filesystem, starts the first user-space process and many other things.


# Processor Modes
CPU modes control how the processor operates: how much memory it can access, whether it can use protected memory, and what kind of instructions it's allowed to run. Modes define the privilege level and capability set. Think of them as different gears in a manual car, you use different ones for different jobs.

These are the CPU modes:
1. [Real mode](#real-mode)
2. [Protected mode](#protected-mode)
3. [Long mode](#long-mode)
4. [Virtual 8086 Mode](#virtual-8086-mode)
5. [System Management Mode](#system-management-mode)

### Real Mode
This is the original mode of the 8086 CPU. The CPU is in real mode when it first turns on. This means that your bootloader will be executed by the CPU in real mode. Keep this in mind. This mode is limited and primitive.

**Characteristics:**
+ 16-bit registers and instructions.
+ Can only access 1 MB of memory.
+ No memory protection, no multitasking, no privilege levels.
+ Direct access to hardware and BIOS interrupts.

This mode is used by the BIOS and the bootloader (first stage).

### Protected Mode
This mode was introduced with the 80286 CPU. It adds memory protection and multitasking.

**Characteristics:**
+ 32-bit registers and instructions.
+ Can access up to 4 GB of memory.
+ Supports virtual memory via paging.
+ Has 4 privilege levels (rings 0-3).
+ Memory segmentation via the GDT (Global Descriptor Table).
+ Hardware-level protection (can't access kernel memory from user mode).

This mode is used by 32-bit operating systems like Windows XP and older Linux.

### Long Mode
This mode was introduced with AMD64 (x86\_64). This is the 64-bit mode all modern OSes use.

**Characteristics:**
+ 64-bit registers.
+ Access to vast amounts of memory (theoretically 16 exabytes).
+ Uses paging.
+ Supports both 64-bit and 32-bit programs.
+ Uses the same privilege levels as protected mode.

This mode is used by modern 64-bit operating systems like Windows 11 and Linux.

**Note:** Every CPU starts in real mode. Even in 64-bit OSes, the CPU will first start in real mode and will be switched to protected mode then long mode based on the OS.

### Virtual 8086 Mode
This is a hybrid mode in protected mode that allows running real-mode programs in a protected environment. It was introduced with the 80386 CPU.

**Characteristics:**
+ Lets you run 16-bit code in protected mode.
+ Frequently used by DOS emulators, bootloaders, and virtualization layers.

You may never use this mode unless you want to support legacy real-mode programs from protected mode.

### System Management Mode
This is a special, hidden mode used by firmware for things like power management and hardware control.

**Characteristics:**
+ Triggered via a *System Management Interrupt (SMI).*
+ Executes code in a hidden memory area called **SMRAM**.
+ Inaccessible to the OS, even ring 0.
+ Often used by BIOS/UEFI and some rootkits.

You don't use this mode in OS dev, unless you're diving deep into firmware-level debugging or security research.
