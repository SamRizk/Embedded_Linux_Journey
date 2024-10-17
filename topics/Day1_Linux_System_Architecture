<p style="color: #4CAF50; font-family: Arial; font-size: 20px;">
  <span style="font-weight: bold;">🖋️ Author:</span> <span style="font-style: italic;">Samir Rizk</span>
</p>

<p style="color: #FF5722; font-family: Arial; font-size: 18px;">
  <span style="font-weight: bold;">📅 Date:</span> <span style="font-style: italic;">15/07/2024</span>
</p>

<p style="color: #2196F3; font-family: Arial; font-size: 15px;">
  <span style="font-weight: bold;">📜 Description:</span> Overview of Linux System Architecture <span style="font-style: italic;"></span>
</p>

<p style="color: #9C27B0; font-family: Arial; font-size: 15px;">
  <span style="font-weight: bold;">©️ Copyright:</span> <span style="font-style: italic;">2024 Samir Rizk. All rights reserved.</span>
</p>

### **Day 1: Linux System Architecture for Embedded Systems**

#### **Objective**: Understand the core components of a Linux system, particularly how they apply to embedded systems.

---

### **1. Overview of Linux System Architecture**

At a high level, a Linux system has four main layers:

1. **Hardware Layer**: This is the actual physical hardware your system runs on (e.g., processor, memory, peripherals).
2. **Kernel Layer**: The core of the operating system. It communicates directly with the hardware and manages resources like CPU, memory, and I/O.
3. **System Libraries & Utilities**: Libraries (like `glibc`) and utilities (commands like `ls`, `ps`) that provide essential functions to user applications.
4. **User Space**: Where all user-level applications (e.g., custom apps, daemons) run.

Here’s a simple diagram to illustrate these components:

```plaintext
 +--------------------------------------------------------+
 |                     User Applications                  |
 +--------------------------------------------------------+
 |                   System Libraries & Utilities         |
 +--------------------------------------------------------+
 |                         Kernel                         |
 +--------------------------------------------------------+
 |                        Hardware                        |
 +--------------------------------------------------------+
```

---

### **2. Linux Kernel in Embedded Systems**

The Linux kernel is the heart of any Linux system, and in embedded systems, it's highly tailored for resource-constrained environments.

#### **Key Functions of the Kernel**:

1. **Process Management**: Controls when and how processes access the CPU.
2. **Memory Management**: Handles the allocation and deallocation of memory for different processes.
3. **Device Drivers**: Acts as a translator between hardware devices and the rest of the system. In embedded systems, drivers are crucial for working with custom peripherals.
4. **File System Management**: Controls how data is stored and retrieved.
5. **Networking**: Manages network interfaces and ensures secure communication.

---

### **3. Boot Process in Embedded Systems**

In an embedded system, the boot process typically follows these steps:

1. **Power-On or Reset**: The hardware is powered up or reset.
2. **Bootloader** (e.g., U-Boot):
   - Initializes basic hardware (e.g., CPU, RAM).
   - Loads the Linux kernel into memory.
3. **Kernel Initialization**:
   - The kernel initializes system drivers and mounts the root filesystem.
4. **Init Process**:
   - The kernel starts the first user-space process (usually `init` or `systemd`).
5. **User Space**:
   - System services (e.g., network manager, logging) start up, and finally, the system is ready to run user applications.

Here’s a simplified diagram of the boot process:

```plaintext
 Power-On -> Bootloader -> Kernel Init -> Init Process -> User Applications
```

---

### **4. File Systems in Embedded Systems**

File systems in embedded systems can differ from those used in general-purpose Linux systems due to limited storage space. Common file systems for embedded systems:

- **ext4**: The most common file system for Linux.
- **SquashFS**: A compressed, read-only file system that is often used in embedded devices due to its efficiency.
- **JFFS2/UBIFS**: Flash-based file systems, often used for NAND/NOR flash memory.

#### **Hands-On Task**:
1. On your development board (or virtual machine), check which file systems are currently supported by the kernel:
   ```bash
   cat /proc/filesystems
   ```

2. Create a SquashFS image to understand how compressed file systems work:
   ```bash
   mksquashfs /path/to/folder /path/to/image.squashfs
   ```

---

### **5. Device Trees and Hardware Interaction**

Embedded systems often use device trees to describe the hardware to the kernel. The device tree specifies things like available peripherals (GPIOs, I2C, SPI, etc.), memory layout, and more.

- **Device Tree Blob (DTB)**: A binary representation of the hardware description, loaded by the bootloader.
  
#### **Hands-On Task**:
1. If you have a board like a Raspberry Pi or BeagleBone, check out its device tree:
   ```bash
   cat /proc/device-tree/model
   ```

2. Explore the contents of the device tree:
   ```bash
   ls /proc/device-tree
   ```

---

### **6. Yocto Project Introduction**

The **Yocto Project** is a popular build system for creating custom Linux distributions for embedded systems. It allows you to:

- Build a tailored Linux OS with only the necessary components for your hardware.
- Package your application into a reproducible build environment.

We’ll cover Yocto in more detail in later lessons, but here’s a quick overview of its components:
  
- **Bitbake**: The build tool that compiles the code and packages.
- **Recipes**: Define how software is built.
- **Layers**: Different layers provide configurations for different hardware or software stacks.

---

### **7. Practical Hands-On: Set Up a Simple Embedded Linux System**

For today’s hands-on, let’s try setting up a basic embedded Linux system on a Raspberry Pi or similar development board:

#### **Steps**:
1. **Get a Pre-Built Image**: Download an embedded Linux image (e.g., Raspbian or Yocto-based image).
2. **Burn the Image**: Use a tool like **Balena Etcher** to burn the image onto an SD card.
3. **Boot the System**: Insert the SD card into your board and power it up.
4. **Explore the System**:
   - Check the running processes: `ps -aux`
   - Explore the mounted file systems: `df -h`
   - Explore the device tree: `ls /proc/device-tree/`

---

### **Conclusion**

Today’s lesson provided an introduction to the **Linux System Architecture** in the context of embedded systems. We covered the kernel’s role, the boot process, file systems, and gave a quick peek at Yocto for building custom embedded Linux systems.

#### **Next Steps**:
- In the next lesson, we’ll dive deeper into the **boot process**, exploring the bootloader and kernel configuration in more detail.
- Continue exploring the system you set up today, focusing on the interaction between the hardware and the Linux kernel.

---
