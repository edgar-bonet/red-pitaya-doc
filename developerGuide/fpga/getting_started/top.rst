
.. _fpga_programming_environment:

###########################################
Getting Started with FPGA Development
###########################################

This section guides you through setting up your FPGA development environment and creating your first Red Pitaya FPGA project. For current Red Pitaya OS releases, use Vivado 2025.1 with Vitis 2025.1. Legacy SDK 2019.1 documentation is kept for older OS branches.

.. note::

    **Toolchain by OS version**

    * **OS 3.00 or higher:** Vivado 2025.1 + Vitis 2025.1 (current flow)
    * **OS 1.04 - 2.00:** Vivado 2020.1 + SDK 2019.1 (legacy flow)

**What you'll find here:**

* **Introduction** - Understand Red Pitaya FPGA architecture, required tools, and development workflow
* **Vivado and Vitis Installation** - Current installation flow for FPGA and ARM software development
* **SDK Installation (Legacy)** - SDK 2019.1 setup for older OS branches
* **Project Creation** - Create your first FPGA project from scratch
* **Modify Existing Projects** - Learn to customize existing Red Pitaya projects
* **Simulation** - Verify your designs with behavioral simulation before hardware deployment
* **Reprogram FPGA** - Load bitstreams to Red Pitaya and test your designs
* **Copy Projects** - Set up project templates and workflows
* **SDK Project Creation** - Create ARM software projects that interface with your FPGA design

.. toctree::
    :maxdepth: 1

    intro.rst
    vivado_install.rst
    sdk_install.rst
    project_creation.rst
    modify_project.rst
    simulation.rst
    reprogram_fpga.rst
    copy_project.rst
    project_creation_sdk.rst
..    new_project.rst

