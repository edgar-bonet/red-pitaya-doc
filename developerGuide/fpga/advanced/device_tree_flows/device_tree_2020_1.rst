.. _device_tree_2020_1:

############################################################
Device Tree Generation (Vivado 2020.1 + SDK 2019.1)
############################################################

.. warning::

    This flow is kept for backward compatibility with older Red Pitaya builds.
    For new development, use :ref:`device_tree_2025_1`.

This page documents the legacy HSI-based device tree generation flow built around:

- Vivado 2020.1
- Xilinx SDK 2019.1
- Local clone of Device Tree Xilinx sources

Legacy default device tree source version:

- ``DTS_VER=2017.2``

.. contents:: Table of Contents
    :local:
    :depth: 1
    :backlinks: top

|

Prerequisites
================

Legacy flow requirements:

- Vivado 2020.1
- Xilinx SDK 2019.1 (``xsct``)
- Device Tree Xilinx repository checkout

.. important::

    This flow expects legacy HSI scripts and directory conventions.

|

Automatic Download (Full Ecosystem Build)
==========================================

When building the full Red Pitaya ecosystem with the main repository ``Makefile.x86``, the Device Tree Xilinx repository 
is downloaded automatically:

.. code-block:: bash

    # In the main RedPitaya repository
    make -f Makefile.x86

The Makefile downloads the archive using:

.. code-block:: makefile

    DTREE_URL ?= https://github.com/Xilinx/device-tree-xlnx/archive/$(DTREE_TAG).tar.gz

|

Manual Setup (Standalone FPGA Build)
=====================================

When using standalone legacy FPGA scripts, provide Device Tree Xilinx manually:

.. code-block:: bash

    # Navigate to RedPitaya-FPGA directory
    cd RedPitaya-FPGA

    # Create tmp directory if it doesn't exist
    mkdir -p tmp
    cd tmp

    # Clone Device Tree Xilinx repository
    git clone https://github.com/Xilinx/device-tree-xlnx device-tree-xlnx-xilinx-v2017.2
    cd device-tree-xlnx-xilinx-v2017.2
    git checkout xilinx-v2017.2

.. note::

    Common legacy version is ``2017.2``, but this can be changed with ``DTS_VER``.
    The checkout directory name must follow ``device-tree-xlnx-xilinx-v{version}``.

|

From FPGA Build Process
=========================

In the legacy flow, device tree files are generated from the Vivado hardware definition using HSI.

.. code-block:: bash

    cd RedPitaya-FPGA
    make PRJ=stream_app MODEL=Z10

This flow will:

1. Generate the Vivado hardware design using Vivado 2020.1
2. Export hardware definition ``prj/{project}/sdk/red_pitaya.sysdef``
3. Run ``xsct red_pitaya_hsi_dts.tcl``
4. Place generated DTS files in ``prj/{project}/sdk/dts/``

Legacy Makefile command used for DTS generation:

.. code-block:: bash

    xsct red_pitaya_hsi_dts.tcl $(PRJ) DTS_VER=$(DTS_VER) MODEL=$(MODEL)

Legacy version override example:

.. code-block:: bash

    make PRJ=stream_app MODEL=Z10 DTS_VER=2018.1

For full build details, see the :rp-github:`Makefile <RedPitaya-FPGA/blob/2.07-48/Makefile>` in one of the older RedPitaya-FPGA repository releases.
The link leads to the OS 2.07-48 makefile. For other releases, please select a different release or tag in the GitHub repository.

|

Understanding the DTS Script
=============================

The legacy ``red_pitaya_hsi_dts.tcl`` script flow is based on HSI and performs the following:

1. Opens the hardware design from ``sdk/red_pitaya.sysdef``
2. Sets repository path to ``tmp/device-tree-xlnx-xilinx-v{DTS_VER}``
3. Creates a device tree software design for ``ps7_cortexa9_0``
4. Sets kernel version and overlay configuration
5. Generates DTS output in ``sdk/dts/``

Typical command sequence:

.. code-block:: tcl

    hsi open_hw_design $path_sdk/red_pitaya.sysdef
    hsi set_repo_path ../../../tmp/device-tree-xlnx-xilinx-v$ver/
    hsi create_sw_design device-tree -os device_tree -proc ps7_cortexa9_0
    hsi set_property CONFIG.kernel_version $ver [hsi get_os]
    hsi set_property CONFIG.dt_overlay true [hsi get_os]
    hsi generate_target -dir $path_sdk/dts

This is the compatibility method preserved for older pipelines.

|

Related Links
===============

- :ref:`device_tree` - Parent device tree page (compilation, loading, troubleshooting)
- :ref:`device_tree_2025_1` - Current flow for new development
- :ref:`fpga_install_sdk` - SDK installation reference
