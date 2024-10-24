### **Day 5: Understanding Device Trees in Embedded Linux**

#### **Introduction to Device Trees**

In embedded Linux systems, hardware can vary significantly from platform to platform. To make Linux more adaptable and modular across different hardware, the **Device Tree (DT)** was introduced. The Device Tree is a data structure for describing the hardware components of a system, including their interconnections. 

Device Trees allow the Linux kernel to remain hardware-agnostic, meaning that instead of hard-coding hardware details into the kernel, the details are provided separately in the form of a **Device Tree Blob (DTB)**.

A **Device Tree Source (DTS)** file is compiled into a **Device Tree Blob (DTB)**, which the Linux kernel reads during the boot process to understand the hardware configuration.

---

### **Why Use Device Trees?**

- **Platform Independence**: It allows the same Linux kernel to run on different hardware platforms by simply providing different device tree files.
- **Hardware Abstraction**: Keeps hardware details outside of the kernel, making the kernel more maintainable and flexible.
- **Modularity**: Makes it easier to add support for new hardware without modifying the kernel.

---

### **Device Tree Structure**

A Device Tree is organized in a tree-like structure with nodes and properties:

1. **Nodes**: Represent hardware devices or components (e.g., CPU, memory, buses).
2. **Properties**: Each node has key-value pairs representing specific attributes of that hardware component (e.g., memory address, interrupt number).

Here’s an example of a simple device tree source (DTS) file:

```dts
/dts-v1/;

/ {
    model = "Example Board";
    compatible = "example,board";

    memory {
        device_type = "memory";
        reg = <0x80000000 0x2000000>;
    };

    cpus {
        #address-cells = <1>;
        #size-cells = <0>;

        cpu@0 {
            device_type = "cpu";
            compatible = "arm,cortex-a7";
            reg = <0>;
        };
    };

    uart@10000000 {
        compatible = "ns16550a";
        reg = <0x10000000 0x100>;
        interrupt-parent = <&intc>;
        interrupts = <5>;
    };

    intc: interrupt-controller {
        compatible = "arm,cortex-a7-gic";
        #interrupt-cells = <3>;
    };
};
```

#### **Key Sections**:
- **Root Node (`/`)**: The base node of the tree, representing the system.
- **Memory Node (`memory`)**: Describes the system memory with start address and size.
- **CPU Node (`cpus`)**: Describes the CPUs in the system.
- **Peripheral Node (`uart@10000000`)**: Describes the UART controller at address `0x10000000` with an interrupt.

Each node has properties that describe the hardware’s specific configuration, such as registers (`reg`), interrupts, and compatible device drivers (`compatible`).

---

### **How Device Trees Are Used**

The kernel relies on device trees to understand the hardware it’s running on. When the system boots:

1. The bootloader (like U-Boot) loads the Linux kernel and provides the DTB file.
2. The kernel parses the DTB to determine the hardware components present on the system.
3. Drivers in the kernel are matched with the devices specified in the Device Tree through the `compatible` property.

For example, in the above DTS file, the UART device has `compatible = "ns16550a"`, which tells the kernel to use the driver compatible with the NS16550A UART controller.

---

### **Understanding Device Tree Compilation**

Device Tree Source files (.dts) need to be compiled into a Device Tree Blob (.dtb) format that the kernel can read. This compilation is done using the `dtc` (Device Tree Compiler) tool.

#### **Steps to Compile a Device Tree Source File**:

1. **Install the Device Tree Compiler (DTC)**:
   ```bash
   sudo apt-get install device-tree-compiler
   ```

2. **Compile the DTS to DTB**:
   ```bash
   dtc -I dts -O dtb -o example_board.dtb example_board.dts
   ```

   - `-I dts`: Specifies that the input format is Device Tree Source.
   - `-O dtb`: Specifies that the output format is Device Tree Blob.
   - `-o example_board.dtb`: The output file name.

3. **Using U-Boot to Load the Device Tree**:
   U-Boot typically loads the Linux kernel and the Device Tree Blob file during the boot process. The `bootm` command can be used with the address of the DTB:
   ```bash
   bootm 0x80000000 - 0x81000000
   ```
   - `0x80000000`: The address of the Linux kernel.
   - `0x81000000`: The address of the DTB file.

---

### **Common Use Cases for Device Trees**

1. **Adding New Hardware Support**: When introducing new hardware, you only need to modify or add a new DTS file without changing the kernel code.
   
2. **Customizing Peripherals**: For example, customizing GPIOs, UART, or I2C controllers by adjusting their configurations in the Device Tree.

3. **Board-Specific Configuration**: Different embedded systems have specific configurations, like clock rates, memory mapping, or interrupt assignments. The Device Tree abstracts these details from the kernel.

---

### **Device Tree Overlays**

Device Tree Overlays provide a way to dynamically modify or extend a base Device Tree at runtime. This is particularly useful in cases where a system has hot-swappable or modular hardware.

For example, on development boards like the Raspberry Pi, a base Device Tree describes the core hardware, while overlays are used to add support for peripherals like displays, sensors, or add-on boards.

---

### **Hands-On Example: Adding a Peripheral**

Let’s walk through an example of adding an I2C device (an EEPROM) to a system using the Device Tree.

1. **Original Device Tree (simplified)**:
   ```dts
   i2c@10003000 {
       compatible = "i2c-bus";
       reg = <0x10003000 0x1000>;
       #address-cells = <1>;
       #size-cells = <0>;

       eeprom@50 {
           compatible = "at24";
           reg = <0x50>;
       };
   };
   ```

2. **Explanation**:
   - `i2c@10003000`: The I2C controller at address `0x10003000`.
   - `eeprom@50`: The EEPROM device with I2C address `0x50`, compatible with the `at24` driver.

This DTS fragment adds an I2C controller and an EEPROM device, which will allow the kernel to load the necessary drivers and interact with the EEPROM.

---

### **Debugging Device Trees**

When dealing with Device Trees, debugging is an important skill. Here are a few tips for troubleshooting:

- **Check dmesg logs**: The kernel logs will show whether a device from the Device Tree was successfully initialized or not. Look for errors related to device binding:
   ```bash
   dmesg | grep 'OF'
   ```
   
- **Check the compiled DTB**: You can decompile a Device Tree Blob back into a human-readable format using `dtc`:
   ```bash
   dtc -I dtb -O dts -o decompiled.dts compiled.dtb
   ```

- **Use `of_overlay` and `of_reserved_mem`**: These kernel debug options can help provide more insight into the Device Tree processing in the kernel.

---

### **Key Takeaways**

- Device Trees provide a structured way to describe hardware for the Linux kernel, making embedded Linux systems more portable and modular.
- U-Boot uses the DTB to pass hardware information to the Linux kernel during boot.
- Learning to read, write, and debug Device Trees is a crucial skill for embedded Linux developers, as most modern systems use them to handle hardware configuration.

