.. _device_tree_2025_1:

############################################################
Device Tree Generation (Vivado 2025.1 + Vitis 2025.1)
############################################################

This page documents the current Red Pitaya device tree generation flow based on:

- Vivado 2025.1
- Vitis 2025.1 (``xsct``)

For legacy compatibility (Vivado 2020.1 + SDK 2019.1), see :ref:`device_tree_legacy_2020_1`.

.. contents:: Table of Contents
    :local:
    :depth: 1
    :backlinks: top

|

Prerequisites
================

Install matching Xilinx tools and ensure ``xsct`` is available in your PATH.

Required versions:

- Vivado 2025.1
- Vitis 2025.1

.. important::

    Use matching Vivado and Vitis versions for stable results.

|

Automatic Download (Full Ecosystem Build)
==========================================

When building the complete Red Pitaya ecosystem via the main repository ``Makefile.x86``, the device tree source repository is 
handled by the ecosystem build flow.

|

Manual Setup (Standalone FPGA Build)
=====================================

For current standalone FPGA builds, no manual local checkout of ``device-tree-xlnx`` is required.
The ``red_pitaya_hsi_dts.tcl`` script uses ``createdts`` with ``-git-branch xlnx_rel_v{DTS_VER}``.

|

From FPGA Build Process
=========================

The standard build flow automatically generates the device tree from the exported XSA hardware platform.

.. code-block:: bash

    cd RedPitaya-FPGA
    make PRJ=stream_app MODEL=Z10

This flow will:

1. Build the hardware design in Vivado 2025.1
2. Export ``prj/{project}/sdk/red_pitaya.xsa``
3. Run ``xsct red_pitaya_hsi_dts.tcl``
4. Place generated DTS files in ``prj/{project}/sdk/dts/``

Makefile command used for DTS generation:

.. code-block:: bash

    xsct red_pitaya_hsi_dts.tcl $(PRJ) DTS_VER=$(DTS_VER) MODEL=$(MODEL)

Default device tree source version:

- ``DTS_VER=2025.1``

Example override:

.. code-block:: bash

    make PRJ=stream_app MODEL=Z10 DTS_VER=2025.1

For full build details, see the `Makefile <https://github.com/RedPitaya/RedPitaya-FPGA/blob/master/Makefile>`_ in the RedPitaya-FPGA repository.

|

Understanding the DTS Script
=============================

The ``red_pitaya_hsi_dts.tcl`` script in RedPitaya-FPGA uses the ``createdts`` flow and performs the following:

1. Reads hardware platform from ``sdk/red_pitaya.xsa``
2. Uses ``DTS_VER`` to select branch suffix ``xlnx_rel_v{DTS_VER}``
3. Generates DTS output with overlay support enabled
4. Copies generated files to ``sdk/dts/``

Core command used by the script:

.. code-block:: tcl

    createdts -hw $xsa_file -platform-name redpitaya_platform -git-branch xlnx_rel_v$ver -overlay -out $output_dir

For complete implementation details, see `red_pitaya_hsi_dts.tcl <https://github.com/RedPitaya/RedPitaya-FPGA/blob/master/red_pitaya_hsi_dts.tcl>`_.

|

Related Links
===============

- :ref:`device_tree` - Parent device tree page (compilation, loading, troubleshooting)
- :ref:`overlay_util` - Quick reference for overlay script
- :ref:`fpga_advanced_loading` - Comprehensive FPGA and device tree reprogramming guide
