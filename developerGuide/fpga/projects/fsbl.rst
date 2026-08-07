.. _fpga_project_fsbl:

######################
FPGA fsbl project
######################

The ``fsbl`` project is a build-support project used for FSBL and U-Boot platform artifacts.
It is conceptually similar to ``barebones``, but enables fewer peripherals/settings because U-Boot does not require the full Linux platform set.
It is not a general application image like ``v0.94`` or ``stream_app``.

.. contents:: Table of Contents
    :local:
    :depth: 1
    :backlinks: top

|

Purpose
----------

Use ``fsbl`` when the flow requires:

* XSA/FSBL generation for boot packaging
* U-Boot-oriented platform and handoff artifacts
* synchronized hardware/software boot handoff outputs

In practice, this project is mainly consumed by Make targets rather than loaded as a user-facing runtime image.

|

How it fits the build flow
---------------------------

The root ``Makefile`` includes dedicated targets such as:

* ``fsbl_build``
* ``fsbl_dts``

These targets run Vivado/XSCT Tcl scripts to produce FSBL and U-Boot-related platform outputs.

Unlike ``barebones``, which provides the full Linux device tree base, ``fsbl`` stays focused on boot-stage needs.

The ``prj/fsbl/dts`` content includes boot-relevant platform fragments and includes.

|

Project structure
------------------

In ``prj/fsbl/``:

* ``ip/system.tcl`` - block-design definition used in platform generation
* ``rtl/`` - project top-level RTL wrapper
* ``dts/`` - boot-stage device-tree support fragments for FSBL and U-Boot integration

|

When to modify
---------------

Modify ``fsbl`` only if you are changing low-level boot/platform behavior, for example:

* peripheral bring-up requirements
* handoff or boot-stage device-tree content
* platform generation scripts and dependencies

For Linux platform-level base work, use ``barebones``.
For application-level feature development in the ecosystem, start from a ``v0.94``-style project.

|

Code architecture (modules)
----------------------------

The top-level source ``prj/fsbl/rtl/red_pitaya_top.sv`` is intentionally minimal and mirrors the barebones style:

* ``system`` - block-design wrapper around PS/DDR platform interfaces.

Most FSBL-specific behavior comes from build scripts and DTS composition rather than rich PL data-path logic.
Compared with ``barebones``, the FSBL/U-Boot platform configuration keeps only the settings needed for boot.

Relevant code/config files:

* ``prj/fsbl/ip/system.tcl`` - platform block-design definition
* ``prj/fsbl/dts/redpitaya.dtsi`` - includes platform-level fragments (Ethernet, I2C, USB, QSPI)
* ``red_pitaya_hsi_fsbl.tcl`` and ``red_pitaya_hsi_fsbl_dts.tcl`` - XSCT-driven generation flow

|

Module connection schematic
----------------------------

.. figure:: img/fsbl/fsbl_module_block_diagram.png
   :alt: FSBL module block diagram
   :align: center

This project focuses on platform handoff and boot artifacts, not runtime signal-processing chains.
