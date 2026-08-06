.. _fpga_intro:

###########################################
Introduction to Red Pitaya FPGA Development
###########################################

Understanding the Red Pitaya Architecture
==========================================

Red Pitaya software is composed of two main parts working together:

1. **FPGA Image** - Hardware logic running on the Xilinx Zynq FPGA that handles high-speed signal processing, data acquisition, and generation (also called the Programmable Logic (PL) side)
2. **Linux Operating System** - Software running on the ARM processor with drivers for interfacing with the FPGA (also called the Processing System (PS) side)

The FPGA and ARM processor are integrated into a single Xilinx Zynq System-on-Chip (SoC), allowing seamless communication between hardware and software components.

|

Required Development Tools
===========================

FPGA development for Red Pitaya requires specific versions of Xilinx tools:

AMD (Xilinx) Vivado (Version depends on OS)
--------------------------------------------

**What it is:** Vivado is the primary development environment for creating and modifying FPGA designs. It includes:

* HDL editor for writing Verilog/VHDL code
* Block diagram editor for graphical design
* Synthesis and implementation tools
* Simulation environment
* Bitstream generation

**When you need it:** Always required for any FPGA development work.

**Current version mapping:**

* **OS 3.00 or higher:** Vivado 2025.1
* **OS 1.04 - 2.00:** Vivado 2020.1

AMD Vitis 2025.1 (Current) / Xilinx SDK 2019.1 (Legacy)
-----------------------------------------------------------

**What it is:** Vitis (current) and SDK (legacy) are used for developing applications that run on the ARM processor and interface with your FPGA design.

**When you need it:** Required only if you're modifying ARM-side software (drivers, APIs, custom applications, FSBL/device-tree related flows). If you're only changing 
the FPGA logic and using existing software, you usually don't need Vitis/SDK.

**Current version mapping:**

* **OS 3.00 or higher:** Vitis 2025.1 (installed together with Vivado in the current installation flow)
* **OS 1.04 - 2.00:** SDK 2019.1 (legacy page kept for backward compatibility)

|

What You Can Accomplish
========================

Depending on your project goals, you can:

**Modify FPGA Design Only**

* Change signal processing algorithms
* Add new hardware peripherals
* Modify timing and data paths
* Customize existing Red Pitaya projects
* **Tools needed:** Vivado only

**Modify ARM Software Only**

* Change application logic
* Add new software features
* Modify APIs and drivers
* **Tools needed:** Vitis (current flow) or SDK (legacy flow), using existing FPGA bitstreams

**Full Custom Development**

* Create entirely new FPGA designs
* Develop matching software drivers
* Integrate custom hardware peripherals
* **Tools needed:** Vivado + Vitis (current flow) or Vivado + SDK (legacy flow)

|

Development Workflow Overview
==============================

The typical FPGA development workflow follows these steps:

1. **Setup Environment** - Install Vivado and, if needed, Vitis (current) or SDK (legacy)
2. **Create/Modify Project** - Start with existing project or create new one
3. **Design** - Write HDL code or modify block diagrams
4. **Simulate** - Verify logic with behavioral simulation (critical step!)
5. **Synthesize** - Convert HDL to hardware gates
6. **Implement** - Place and route design on FPGA
7. **Generate Bitstream** - Create FPGA binary file
8. **Test on Hardware** - Load to Red Pitaya and verify
9. **Integrate Software** - Create/update drivers and applications (if needed)

|

Next Steps
==========

Now that you understand the architecture and tools, proceed with:

1. **Install Vivado and Vitis** - Current flow for OS 3.00 or higher
2. **Use Legacy SDK guide only if needed** - For OS 1.04 - 2.00 maintenance
3. **Create Your First Project** - Follow the project creation guide
4. **Learn Simulation** - Essential for efficient development

The following sections will guide you through each step in detail.

|
