# Assembly

> **Random Quote:** Assembly language is where the hardware meets the code, there’s no closer conversation between man and machine.

Assembly language provides direct control over hardware. It is essential for writing low-level code such as bootloaders, interrupt handlers, and early-stage kernel routines. This roadmap equips you with enough Assembly knowledge to write a bootloader.

## Set up dev env
+ Install your assembler of choice. I recommend NASM.

## Instruction Format
+ Learn Intel's Assembly syntax.
+ Example: `MOV AX, 0x10`

## Data Movement Instructions
+ Learn how to move data around: registers, memory, stack.
+ Instructions: `MOV`, `XCHG`.

## Arithmetic and Logic
+ Learn the following commands: `ADD`, `SUB`, `INC`, `DEC`, `MUL`, `DIV`. `AND`, `OR`, `XOR`, `NOT`, `SHL`, `SHR`.

## Control flow
+ Learn about labels and the `JMP` instruction.
+ Learn about some of the conditional jump instructions like `JC`, `JZ`, `JE`, `JNE`, etc.

## Stack operations
+ Learn about the stack and these instructions: `PUSH`, `POP`, `CALL`, `RET`.

## Interrupts
+ `INT`. Play around with system interrupts then BIOS interrupts.

## Assembler Instructions
+ Learn the following NASM assembler instructions: `BITS`, `ORG`, EQU`, `TIMES`, etc. The rest will be in the notes folder.
