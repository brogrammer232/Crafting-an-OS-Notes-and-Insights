# Welcome to OS Development

> **Random quote:** "If you are not prepared to go all the way, why take the first step?"

So you want to build an operating system?  
Good. You're exactly where you need to be.

## 🧠 What is OS Development?

Operating system development is the art (and chaos) of writing code that runs *without* another OS underneath.  
You're not writing Python scripts on top of Linux — you're writing the *thing* that boots *before* Linux even wakes up.

You'll be dealing with:
- Bare-metal code that boots from BIOS or UEFI
- Memory management without `malloc()`
- Interrupts, timers, and CPU privilege rings
- Building your own `printf()`
- Writing keyboard drivers from scratch
- Designing your own syscall interface
- Bootstrapping your own userland

It’s an ocean of fun, pain, and sleepless nights.  
It’s low-level. It’s messy. It’s humbling.

And it’s **incredibly rewarding**.

## 🔥 Why Bother?

First of all: bragging rights.

Then there's the fact that it forces you to understand how a computer *actually* works.

You stop thinking of a CPU as "just a black box that runs code" and start seeing the real layers — registers, stack frames, interrupt tables, page tables, schedulers, filesystems — all the invisible plumbing that makes modern systems tick.

If you've ever thought high-level programming felt like magic, this repo is your spellbook for how the magic works.

## 💡 How to Use This Repo

- **Start in `notes/`** — that’s where I document what I’ve learned.
- **Check `projects/`** for hands-on experiments you can try.
- **Peek at `tools/`** to get your environment set up right.
- **Dig through `resources/`** for PDFs, diagrams, and external gems.

Everything is written to be clear, practical, and beginner-accessible — because I’m learning this as I go, just like you.

## 👊 Final Words

OS dev isn’t easy, and that’s the point.  
If you’re here, you’ve already got curiosity and stubbornness. That’s all you need to start.

Welcome to the deep end.

Now go write your own OS… and grow a beard in the process.
