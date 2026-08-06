.. _fpga_project_stream_app:

########################
FPGA stream app project
########################

The ``stream_app`` project is the dedicated high-throughput streaming FPGA image in the Red Pitaya repository.
It is intended for continuous or burst transfer of ADC, DAC, and GPIO data through DMA paths, with host-side control through CLI/API streaming tools.

.. contents:: Table of Contents
    :local:
    :depth: 1
    :backlinks: top

|

Purpose
----------

Use ``stream_app`` when the main requirement is moving data efficiently between programmable logic and memory/host software.

Typical use cases:

* ADC capture streaming to memory and then to network clients
* DAC playback streaming from memory/host data
* GPIO data streaming and synchronized digital I/O workflows
* long-running acquisition/generation where deep buffers and DMA control are required

|

Architecture overview
----------------------

Compared to ``v0.94``, this project focuses more on transfer pipelines and less on broad mixed-instrument functionality.

At a high level, the image contains:

* ADC streaming path(s) with trigger, filtering, decimation and DMA write support
* DAC streaming path(s) with DMA read support and playback control
* GPIO streaming path(s) for digital data movement
* control/status registers for event handling, trigger configuration, DMA mode and diagnostics

The register map is split into three chip-select regions:

* ``CS[0]`` - ADC streaming
* ``CS[1]`` - DAC streaming
* ``CS[2]`` - GPIO streaming

For detailed registers, see:

* :ref:`In Dev <regset_in_dev>`

|

Project structure
------------------

In ``prj/stream_app/`` you will typically work with:

* ``rtl/`` - main top-level and streaming logic (including board/variant subdirectories)
* ``ip/`` - block design Tcl and board-specific Tcl helpers
* ``dts/``, ``dts_250/``, ``dts_4ch/`` - device-tree fragments for model-specific variants
* ``sdc/`` - timing constraints per board/model
* ``tbn/`` - simulation testbenches and scripts

|

Board variants and model-specific notes
----------------------------------------

The project is implemented with model-specific variants, including 4-input and 250-12 platforms.

In the current build flow, device-tree include path selection is handled by ``Makefile`` logic based on ``PRJ`` and ``MODEL``/``FPGA_VERSION``.
For ``stream_app``, this determines whether ``dts``, ``dts_250`` or ``dts_4ch`` content is used.

|

Build and verification flow
----------------------------

Typical commands:

* ``make project PRJ=stream_app MODEL=Z20_250``
* ``make PRJ=stream_app MODEL=Z20_250``
* ``make dts PRJ=stream_app MODEL=Z20_250``

Recommended validation:

* confirm DMA start/stop and buffer status registers behave as expected
* verify trigger and pre/post sample behavior in your selected acquisition mode
* test sustained throughput with your target host interface and software stack

|

Code architecture (modules)
----------------------------

The main top-level source is ``prj/stream_app/rtl/red_pitaya_top.sv``. The design is built from reusable modules connected around AXI4-Stream and system-bus interfaces.

Core modules in ``red_pitaya_top``:

* ``red_pitaya_ps`` - processing-system wrapper (DDR, MIO, clocks, IRQ, AXI HP stream links).
* ``red_pitaya_pll`` - generates internal ADC/DAC/serial/PDM clocks and lock status.
* ``sys_bus_interconnect`` - address decoder and system-bus fanout to functional blocks.
* ``sys_bus_stub`` - safe terminators for unused system-bus regions.
* ``old_id`` - build/version identification registers.
* ``muxctl`` - loopback and path-selection control for stream routing.
* ``cts`` - global timestamp counter used by capture modules.

Acquisition/generation modules:

* ``scope_top`` (per ADC channel) - trigger, decimation/filtering, capture pipeline, DMA stream output.
* ``old_asg_top`` (two ASG channels) - waveform generation and trigger/IRQ logic.
* ``old_asg_top`` (logic generator instance ``lg``) - digital pattern generation for extension-path output.
* ``old_la_top`` - logic-analyzer capture path from extension inputs to DMA stream.

Stream utility modules:

* ``axi4_stream_mux`` - selects source stream for DAC path.
* ``axi4_stream_pas`` - pass-through/format bridge blocks used in stream paths and loopback.

Auxiliary output modules:

* ``sys_reg_array_o`` + ``pdm`` - register-controlled PDM DAC outputs.
* optional ``sys_reg_array_o`` + ``pwm`` - enabled only when ``ENABLE_PWM`` is defined.

Block-design IP used by ``prj/stream_app/ip/system.tcl`` includes:

* ``rp_oscilloscope``
* ``rp_dac``
* ``rp_gpio``
* ``rp_concat``
* Zynq PS7 and standard Xilinx glue IP

|

Module connection schematic
----------------------------

.. figure:: img/stream_app/streamapp_module_block_diagram.png
   :alt: Stream App module block diagram
   :align: center

