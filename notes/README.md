# Welcome to OS Development

> **Random quote:** If you are not prepared to go all the way, why take the first step?

So, you want to build an operating system?  
Good. You're exactly where you need to be.

## What is OS Development?

Operating system development is the art (and chaos) of writing code that runs without an OS underneath.  
You're not writing Python scripts on top of Linux, you're writing the thing that boots before Linux even wakes up.

You'll be dealing with:

- Bare-metal code that boots from BIOS or UEFI
- Memory management without `malloc()`
- Interrupts, timers, and CPU privilege levels
- Building your own standard library
- Writing drivers from scratch
- Designing your own syscall interface
- Bootstrapping your own userland

It’s an ocean of fun, pain, and sleepless nights.  
It’s low-level. It’s messy. It’s humbling.

And it’s **insanely rewarding**.

## Why Bother?

First: bragging rights.

Second: It forces you to understand how computers actually work.

Building an OS teaches you:

- How hardware and software interact beneath the abstractions
- How CPUs, memory, and devices communicate
- How an operating system juggles control over the entire machine
- Why "files," "threads," and other abstractions are modern miracles

You'll stop seeing a CPU as "just a black box that runs code" and start seeing the layers: registers, stack frames, page tables, interrupt vectors, schedulers, filesystems - all the invisible machinery powering modern systems.

## What you'll need

+ Curiosity about how computers really work
+ Willingness to debug weird, confusing errors
+ A mindset that refuses to quit

## Final Words

OS development isn't easy, and that's the point. If you're here, you've already got the curiosity you need.

Welcome to the deep end.

Now go build your own OS.

## Next Step

Check the [roadmap](../roadmap/README.md) to see where to begin.