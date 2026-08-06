.. _FPGA_install_vivado:

#################################
Installation of Vivado and Vitis
#################################

This installation tutorial is intended for anyone who wants to use the FPGA of the Red Pitaya board.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|


Requirements
=============

You need one of the following on your computer or virtual machine: 

* Ubuntu 18.04 or higher
* Linux Mint OS
* Windows with:
    
    * Archive extract utility (for example, *Winrar* or *7zip*)
    * Windows Subsystem for Linux (recommended)
    * *make* utility (recommended)

Vivado supports both Linux and Windows operating systems. This tutorial will cover the installation process for both operating systems and any 
differences in installation process. MAC users should use a virtual machine with one of the supported operating systems.

Windows users can benefit from using the Windows Subsystem for Linux (WSL) as it is useful for interacting with Red Pitaya and getting access to the 
information from serial console, but it is not required for programming the FPGA. Additionally, having access to the **make** utility will simplify 
the FPGA project build process. For installation instructions, see:

* :ref:`WSL Setup Guide <wsl_setup>`
* :ref:`C++ Compiler and Make Utility Setup <cpp_make_install>`

|

Vivado and Vitis versions
==========================

The version of Vivado and Vitis depends on the Red Pitaya OS version you are using.

.. note::

    It is imperative to use the correct version of Vivado and Vitis for your Red Pitaya OS version. Using an incorrect version may lead to unexpected issues and errors during the FPGA development process.

    **Why not any/up-to-date Vivado Version?**

    The reason is quite simple actually, the automatic project build scripts are written for a specific Vivado/Vitis version and will not work with different versions. 
    It is possible to use a different version of Vivado/Vitis, but you will have to manually create a new project and add all the files, which is not recommended for beginners.


.. tabs::

    .. group-tab:: OS 3.00 or higher

        **Vivado 2025.1 + Vitis 2025.1**

    .. group-tab:: OS 1.04 - 2.00

        **Vivado 2020.1 + SDK 2019.1**

|

Vivado and Vitis installation instructions by version
======================================================

.. toctree::
    :maxdepth: 1

    vivado_install/vivado_2025_1.rst
    vivado_install/vivado_2020_1.rst

|
