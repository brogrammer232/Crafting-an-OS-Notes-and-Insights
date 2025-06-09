# Volatile and Non-Volatile Memory

> **Random Quote:** Perfection is achieved not when there is nothing more to add, but when there is nothing left to take away.

**Memory** refers to components that store data, either temporarily or permanently, for access and processing. Broadly, memory falls into two categories:

+ **Volatile Memory:** Loses data when power is cut.
+ **Non-Volatile Memory:** Retains data even without power.

---

## Volatile Memory

Volatile memory is a type of computer memory that requires power to maintain the stored information. Once the system is powered off, the contents are lost.

### Types of Volatile Memory

+ **RAM (Random Access Memory):**
    - There are two types of RAM:
        * **DRAM (Dynamic RAM):** Needs to be refreshed thousands of times per second. It is slower and cheaper.
        * **SRAM (Static RAM):** Does not need to be refreshed thousands of times per second. It is faster and more expensive. It's also more reliable.

+ **Cache:** This is located on or near the CPU. It is very fast SRAM used to store frequently accessed instructions/data.

### Use Cases

+ Used to temporarily store data for programs that are actively running.
+ They act as high-speed workspace for the CPU (cache and registers).

### Read/Write Speeds

Volatile memory read and write speeds are extremely fast, typically in the range of nanoseconds. For instance, DDR4 RAM has a bandwidth of up to 25.6 GB/s per module.

### Data Retention

As mentioned earlier, everything is wiped when power is lost. This is why unexpected shutdowns lead to data loss if unsaved.

---

## Non-Volatile Memory

Non-volatile memory retains stored data even when not powered. They are mostly used for long-term storage.

### Types of Non-Volatile Memory

+ **ROM (Read-Only Memory):** This is not typically writable during normal operation. It includes pre-programmed data like **firmware/BIOS**.

+ **Flash Memory:** This is electrically erasable and reprogrammable. They are easily writable during normal operation. Examples: SSDs, USB drives, and microcontrollers.

+ **Hard Disk Drives (HDDs):** These store data magnetically on spinning disks. They are therefore affected by strong magnets. They are also easily writable during normal operation.

+ **Optical Discs (CDs/DVDs):** These store data using lasers and pits on a disc surface.

### Use Cases

+ Store firmware, operating systems, and user data on computers.
+ They are also used for long term data storage (personally or on the cloud).

### Read/Write Speeds

Non-volatile memory is a lot slower than volatile memory.

+ SSDs: ~500 MB/s (SATA), and up to 7000 MB/s (NVMe).
+ HDDs: ~80-160 MB/s.
+ ROM: This is the slowest of all. EEPROM ranges around 0.1-1 MB/s while mask ROM averages at 1-10 MB/s. You don't need to know all the different types of ROM for now.

### Data Retention

Non-volatile memory can store data for years or decades, depending on type and wear. They are ideal for storage and archival.

---

## Bonus Concepts You Should Know

+ **EEPROM (Electrically Erasable Programmable ROM)**:
    - Byte-level access.
    - Slower than flash memory.

+ Flash Memory:
    - Block-level access.
    - Faster and cheaper than EEPROM.
    - Used in USB drives, SSDs, and microcontrollers.

+ **Firmware:** This is software embedded into hardware. It is usually store in non-volatile memory like ROM or Flash. It is critical for device startup (e.g., BIOS, UEFI).
