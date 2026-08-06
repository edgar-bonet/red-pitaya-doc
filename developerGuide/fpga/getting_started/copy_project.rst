.. _fpga_copy_project:

###################################
Creating a copy for a new project
###################################

Each project has its own directory under ``prj/``. Generated files can be overwritten when rebuilding the same project name, so keeping a separate copy is recommended.

For example, running the following command when the project already exists (perhaps it is your LED blink project or a new custom FPGA image):

.. code-block:: bash

    make project PRJ=v0.94 MODEL=Z10

Will revert everything in the v0.94 project located in the **RedPitaya-FPGA/prj/v0.94** directory back to the original state, 
which can cause problems when project backups are required.

Here is how you can create a separate project folder for Vivado 2025.1 flow:

#.  Create a new folder called ``new_project`` in **RedPitaya-FPGA/prj/**.
#.  Copy all files from **RedPitaya-FPGA/prj/v0.94** into ``prj/new_project``.
#.  Add or copy your existing *VHDL* or *Verilog* files to ``prj/new_project/rtl``.
#.  Keep or create ``prj/new_project/tbn`` for testbenches.
#.  Open the copied project with the current model-specific launcher:

    * **Linux/Unix-like shell**:

      .. code-block:: bash

          ./open_vivado.sh new_project Z10

    * **Windows CMD/PowerShell**:

      .. code-block:: bat

          open_vivado.bat new_project Z10

#.  Alternatively, use the Makefile project target:

    .. code-block:: bash

        make project PRJ=new_project MODEL=Z10

#.  Build the copied project:

    .. code-block:: bash

        make PRJ=new_project MODEL=Z10

#.  If everything is set up correctly, Vivado and command-line builds should generate outputs in ``prj/new_project/out``.

| 

Legacy Vivado 2020.1 note
==========================

If you are maintaining older OS 1.04 - 2.00 flows, you may still encounter documentation or scripts that refer to older project Tcl patterns.
For those cases, keep using the legacy toolchain documented in :ref:`Vivado 2020.1 installation <FPGA_install_vivado_2020_1>` and :ref:`SDK legacy installation <fpga_install_sdk>`.
For complete legacy checkout and build commands, see :ref:`Legacy Vivado 2020.1 compatibility <fpga_legacy_2020_flow>`.

