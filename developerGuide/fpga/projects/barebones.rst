.. _fpga_project_barebones:

###########################
FPGA barebones project
###########################

The ``barebones`` project is a minimal Red Pitaya FPGA base focused on Linux platform bring-up.
It keeps a common processing-system configuration shared across projects to prevent build conflicts,
while intentionally excluding application-specific ADC/DAC logic.

.. contents:: Table of Contents
    :local:
    :depth: 1
    :backlinks: top

|

Purpose
----------

Use ``barebones`` when you want to:

* start a custom FPGA project from a clean baseline
* reuse the common PS configuration used across the project ecosystem
* build a Linux-capable base and add only the custom modules you need

|

What is included
-----------------

The project contains the basic pieces required to build and boot:

* project top modules in ``rtl/`` (including model-specific tops)
* block-design Tcl in ``ip/system.tcl``
* shared PS configuration in ``ip/ps7_config.tcl``
* base constraints in ``sdc/``
* device-tree sources used to generate the full Linux device tree

This makes ``barebones`` the reference Linux platform project rather than a feature-complete application design.

|

Device-tree handling
---------------------

``barebones`` builds the full device tree used by Linux.

Other FPGA projects normally generate only a project overlay, which is applied on top of the Linux base.
Because of that, ``barebones`` remains the canonical place for full platform-level device-tree definitions.

If your design adds AXI peripherals, update:

* ``prj/barebones/dts/fpga.dts``
* any required model-specific DTS include content

|

FSBL project role
------------------

The FSBL-oriented project is used for FSBL and UBOOT build targets.
It is conceptually similar to ``barebones``, but uses fewer enabled settings because UBOOT does not require all peripherals that Linux uses.

|

Recommended workflow
---------------------

1. If your custom design will be the only project in the system, start from ``prj/barebones`` and adapt the technical content directly.
2. If your project must coexist with the wider ecosystem, base your structure and integration on a ``v0.94``-style project.
3. Add custom RTL/IP and update ``ip/system.tcl`` if needed.
4. Update constraints and device-tree content (full DTS in ``barebones``, overlays in other projects).
5. Build and validate with your target ``MODEL``.

Typical commands:

* ``make project PRJ=barebones MODEL=Z20``
* ``make PRJ=barebones MODEL=Z20``
* ``make dts PRJ=barebones MODEL=Z20``

|

Code architecture (modules)
----------------------------

The core top-level source is ``prj/barebones/rtl/red_pitaya_top.sv``.
This top-level intentionally contains only one major instantiated block:

* ``system`` - Vivado block-design wrapper that connects PS7, DDR, and MIO interfaces.

In other words, ``barebones`` intentionally strips away application modules and keeps only the minimum PS/DDR platform needed to boot Linux and serve as a base.

The original goal was to keep this base as board-independent as possible for universal builds.
In practice, board-specific ADC/DAC differences (initialization flow, bit width, sampling frequencies, and related details) prevent a single shared application implementation.
For that reason, ``barebones`` should remain empty at the application layer.

The block-design content itself is defined by:

* ``prj/barebones/ip/system.tcl``
* ``prj/barebones/ip/ps7_config.tcl``

|

Module connection schematic
----------------------------

.. figure:: img/barebones/barebones_module_block_diagram.png
   :alt: Barebones module block diagram
   :align: center
    
red_pitaya_top (barebones) is intentionally minimal and wraps system only.
