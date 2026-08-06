.. _fpga_projects:

########################
FPGA projects
########################

This section contains information about various FPGA projects available for Red Pitaya. Each project is designed to demonstrate different functionalities and capabilities of the FPGA hardware.

**Related Documentation:**

* :ref:`FPGA Register Maps <fpga_registers>` - Detailed register documentation for each project
* :ref:`FPGA Development <fpga_top>` - General FPGA development guide

.. contents:: Table of Contents
    :local:
    :depth: 1
    :backlinks: top

|

FPGA repository
==================

Before we jump to the projects, let's take a look at the |FPGA GitHub repository| structure.

The repository contains multiple FPGA projects, with either generic functionality or specific functionality related to a particular application.

* Code common to all projects, which mostly contains reusable modules, is directly in the top directory.
* Project-specific code is located inside the ``prj/<project_name>/`` directories.


.. |ug895| replace:: Vivado System-Level Design Entry
.. _ug895: https://www.xilinx.com/support/documentation/sw_manuals/xilinx2017_2/ug895-vivado-system-level-design-entry.pdf


+-------------------+------------------------------------------------------------------+
| Path              | Contents                                                         |
+===================+==================================================================+
| ``archive/``      | Archive of old FPGA bit files compressed in .xz format           |
+-------------------+------------------------------------------------------------------+
| ``brd/``          | Board files (|ug895|_)                                           |
+-------------------+------------------------------------------------------------------+
| ``doc/``          | Documentation (block diagrams, address space, ...)               |
+-------------------+------------------------------------------------------------------+
| ``dts/``          | Device tree source include files                                 |
+-------------------+------------------------------------------------------------------+
| ``ip/``           | Third party IP, for now, Zynq block diagrams                     |
+-------------------+------------------------------------------------------------------+
| ``prj/name``      | Project `name` specific code                                     |
+-------------------+------------------------------------------------------------------+
| ``rtl/``          | Verilog (SystemVerilog) *Register-Transfer Level*                |
+-------------------+------------------------------------------------------------------+
| ``sdc/``          | *Synopsys Design Constraints* contains Xilinx design constraints |
+-------------------+------------------------------------------------------------------+
| ``sim/``          | Simulation scripts                                               |
+-------------------+------------------------------------------------------------------+
| ``tbn/``          | Verilog (SystemVerilog) *test bench*                             |
+-------------------+------------------------------------------------------------------+
| ``Makefile``      | Main Makefile, used to run FPGA-related tools                    |
+-------------------+------------------------------------------------------------------+
| ``*.tcl``         | TCL scripts to be run inside FPGA tools                          |
+-------------------+------------------------------------------------------------------+
| ``*.rst``         | ReStructuredText files for documentation                         |
+-------------------+------------------------------------------------------------------+

|


FPGA projects
====================

All existing projects have either generic functionality or specific functionality related to a particular application as described in the "Application" column.

We recommend using the **0.94** as the *default project*.


+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| Project name      | Description                                                            | Application                      | Status             |
+===================+========================================================================+==================================+====================+
| 0.94              | | Default and most feature-complete Red Pitaya FPGA image. Used as the | | Oscilloscope                   | Active (default)   |
|                   | | baseline for current application support and most custom forks.      | | Signal generator               |                    |
|                   | | In the 2025.1 flow it remains the recommended starting point.        | | Spectrum analyzer              |                    |
|                   | |                                                                      | | Bode analyzer                  |                    |
|                   | |                                                                      | | Impedance analyzer             |                    |
|                   | |                                                                      | | LCR meter                      |                    |
|                   | |                                                                      | | JupyterLab                     |                    |
|                   | |                                                                      | | **Register map:**              |                    |
|                   | |                                                                      | | :ref:`v0.94 <fpga_094_dev>`    |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| stream_app        | | High-throughput streaming project. Supports ADC/DAC/GPIO transfer    | | Data streaming                 | Active             |
|                   | | between PL and DDR memory, and host-to-board streaming workflows.    | | Streaming server / API flows   |                    |
|                   | | Includes board-specific variants (for example 4-input and 250-12).   | |                                |                    |
|                   | |                                                                      | | **Register maps:**             |                    |
|                   | |                                                                      | | :ref:`In Dev <regset_in_dev>`  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| logic             | | Logic-analyzer oriented project with DMA-based capture to DDR.       | Logic analyzer                   | Active             |
|                   | | Focuses on digital acquisition and protocol analysis workflows.      |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| barebones         | | Linux platform base with shared PS configuration used across         | Foundation for Linux system      | Active             |
|                   | | projects. Builds the full Linux device tree; other projects          |                                  |                    |
|                   | | typically provide overlays only. Keeps application layer             |                                  |                    |
|                   | | intentionally minimal/empty.                                         |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| pyrpl             | | Third-party oriented image for PyRPL workflows, including lock-in,   | PyRPL / control loops            | Community          |
|                   | | IQ, filter, and feedback-control related DSP blocks.                 | (advanced DSP/control)           |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| fsbl              | | Build-support project for FSBL and U-Boot artifacts (XSA/FSBL flow). | Boot and platform artifacts      | Build support      |
|                   | | Similar to barebones but with fewer enabled peripherals/settings,    |                                  |                    |
|                   | | because U-Boot does not require the full Linux platform scope.       |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| Examples          | | Collection of standalone educational Vivado examples (for example    | Learning and quick demos         | Legacy             |
|                   | | LED/GPIO/VGA exercises). Useful for training and quick experiments.  |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+

The table above reflects projects currently present in ``prj/`` on the master branch of the FPGA repository. Older projects from previous releases may still be available in historical tags.

|

Legacy projects
----------------

These projects are no longer in the FPGA repository and are not actively maintained. They may be incompatible with the latest hardware revisions or software versions. 
Use them only for reference or if you are maintaining older systems.

+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| Project name      | Description                                                            | Application                      | Status             |
+===================+========================================================================+==================================+====================+
| 0.93              | | The original Red Pitaya FPGA release with all original bugs.         |                                  | Legacy             |
|                   | | For deprecated application backward compatibility only.              |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| classic           | | 1. Most of the code is rewritten in SystemVerilog.                   |                                  | Legacy             |
|                   | | 2. The GPIO and LED registers were removed from the housekeeping     |                                  |                    |
|                   | |    section; instead, the GPIO controller inside the PL is used. This |                                  |                    |
|                   | |    allows Linux kernel features to be used for GPIO (IRQ, SPI, I2C   |                                  |                    |
|                   | |    and 1-Wire) and LEDs (triggers).                                  |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| axi4lite          | | This image is intended for testing various AXI4 bus implementations. |                                  | Legacy             |
|                   | | It contains a Vivado Integrated Logic Analyser (ILA) for observing   |                                  |                    |
|                   | | and reviewing the performance of the bus implementation.             |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| tft               | | The TFT FPGA image supports connection to TFT displays, with         |                                  | Legacy             |
|                   | | instructions available :ref:`here <tft_displays>`. Compatible with   |                                  |                    |
|                   | | 0.97 and 0.98 OS versions.                                           |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+
| mercury           | | The old image used by Jupyter Notebook application. Replaced by      | Jupyter Notebook                 | Legacy             |
|                   | | :ref:`Python API commands <C&Py_API>` in the latest OS versions.     |                                  |                    |
|                   |                                                                        |                                  |                    |
+-------------------+------------------------------------------------------------------------+----------------------------------+--------------------+

|


In-depth project descriptions
==============================

The following pages cover active projects in the current repository layout. PyRPL and Examples are intentionally not covered here.

.. toctree::
    :maxdepth: 1

    v0_94.rst
    stream_app.rst
    logic.rst
    barebones.rst
    fsbl.rst

|

Board compatibility
=====================

Not all projects are compatible with all Red Pitaya boards. The table below shows the compatibility of each project with the different board versions.

The following table shows which projects are available on which boards.

.. include:: fpga_project_table.inc

.. include:: fpga_project_flags.inc


.. note::

    Legacy projects are not actively maintained and may not be compatible with the latest hardware revisions or software versions. It is recommended to use active projects for new developments.

.. substitutions

.. Board references for FPGA compatibility table
.. |125-10| replace:: :ref:`STEMlab 125-10 <top_125_10>`
.. |125-14| replace:: :ref:`STEMlab 125-14 <top_125_14>`
.. |125-14_z7020| replace:: :ref:`STEMlab 125-14 Z7020 <top_125_14_Z7020_LN>`
.. |125-14_gen2| replace:: :ref:`STEMlab 125-14 Gen 2 <top_125_14_gen2>`
.. |125-14_pro_gen2| replace:: :ref:`STEMlab 125-14 PRO Gen 2 <top_125_14_pro_gen2>`
.. |125-14_pro_z7020| replace:: :ref:`STEMlab 125-14 PRO Z7020 Gen 2 <top_125_14_pro_z7020_gen2>`
.. |125-14_TI| replace:: :ref:`STEMlab 125-14 TI <top_125_14_TI>`
.. |65-16_TI| replace:: :ref:`STEMlab 65-16 TI <top_65_16_TI>`
.. |122-16| replace:: :ref:`SDRlab 122-16 <top_122_16>`
.. |125-14_4in| replace:: :ref:`STEMlab 125-14 4-Input <top_125_14_4-IN>`
.. |250-12| replace:: :ref:`SIGNALlab 250-12 <top_250_12>`

.. |FPGA GitHub repository| replace:: `FPGA GitHub repository <https://github.com/RedPitaya/RedPitaya-FPGA>`__
