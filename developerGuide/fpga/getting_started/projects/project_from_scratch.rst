.. _fpga_project_from_scratch:
.. _fpga_config_files:

#############################################
Creating a Custom Project from Scratch
#############################################

This guide explains how to create a custom Red Pitaya FPGA project when you are not relying on the standard template workflow.

.. note::

    This guide currently focuses on constraint file selection. In the future, we plan on expanding it to cover the full new project creation process in Vivado or
    at least the important steps for selecting the correct zynq part number and other tips for creating a new project from scratch.

Unlike :ref:`project_creation <fpga_create_project>`, where the existing scripts and project structure already select most files for you, this page focuses on:

- selecting the correct constraint files (``.xdc``)
- selecting model-dependent configuration files
- structuring a new ``prj/<your_project>/`` directory

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Why This Matters
=================

In RedPitaya-FPGA, build results depend on both:

- ``PRJ`` (project directory under ``prj/``)
- ``MODEL`` (board/model flag)

When creating a project from scratch, using the wrong constraint set usually causes:

- pin assignment errors
- I/O standard mismatches
- timing failures
- hardware behavior that does not match your board

|

Constraint Sources in RedPitaya-FPGA
=====================================

Constraint handling is layered.

1. **Global/model constraints** from repository root ``sdc/``
2. **Project-specific constraints** from ``prj/<PRJ>/sdc/``
3. **IP-level packaged constraints** (when applicable, from IP metadata)


Global/model constraints (root ``sdc/``)
----------------------------------------

The root ``sdc/`` directory contains model-specific base constraints shared across projects.

Examples from current master:

- ``sdc/red_pitaya.xdc``
- ``sdc/red_pitaya_z20.xdc``
- ``sdc/red_pitaya_z20_14.xdc``
- ``sdc/red_pitaya_4ADC.xdc``
- ``sdc/red_pitaya_G2.xdc``
- ``sdc/red_pitaya_z20_250.xdc``
- ``sdc/red_pitaya_z20_ll.xdc``


Project-specific constraints (``prj/<PRJ>/sdc/``)
-------------------------------------------------

Each project can provide additional or overriding constraints under its own ``sdc/`` folder.

Examples:

- ``prj/v0.94/sdc/``
- ``prj/stream_app/sdc/``
- ``prj/logic/sdc/``
- ``prj/barebones/sdc/``

In the standard model scripts (``red_pitaya_vivado_<MODEL>.tcl``), project-level constraints are added after root constraints when matching files exist.


IP-level constraints
--------------------

Some IP cores include their own internal constraint fragments (for example out-of-context timing constraints) through IP packaging metadata.

Treat these as supplemental. Your board pin mapping and top-level timing intent should still be controlled by the root/project ``.xdc`` files above.

|

MODEL to Base Constraint Mapping
=================================

In current RedPitaya-FPGA scripts, base ``.xdc`` selection is tied to ``MODEL``:

+------------+--------------------------------------+
| MODEL      | Base root constraint file            |
+============+======================================+
| ``Z10``    | ``sdc/red_pitaya.xdc``               |
+------------+--------------------------------------+
| ``Z20``    | ``sdc/red_pitaya_z20.xdc``           |
+------------+--------------------------------------+
| ``Z20_14`` | ``sdc/red_pitaya_z20_14.xdc``        |
+------------+--------------------------------------+
| ``Z20_4``  | ``sdc/red_pitaya_4ADC.xdc``          |
+------------+--------------------------------------+
| ``Z20_G2`` | ``sdc/red_pitaya_G2.xdc``            |
+------------+--------------------------------------+
| ``Z20_ll`` | ``sdc/red_pitaya_z20_ll.xdc``        |
+------------+--------------------------------------+
| ``Z20_250``| ``sdc/red_pitaya_z20_250.xdc``       |
+------------+--------------------------------------+

Special case for ``Z20_250``:

- If ``HWID`` is provided, scripts can select ``sdc/red_pitaya_z20_250_<HWID>.xdc`` (example: ``red_pitaya_z20_250_v1r0.xdc``).

|

Recommended Project Layout
===========================

For a custom project, start with:

.. code-block:: text

    prj/<your_project>/
    ├── rtl/
    ├── ip/
    ├── sdc/
    ├── dts/
    └── tbn/          (optional)

If your project supports multiple models, keep model-specific ``.xdc`` file names aligned with the naming convention expected by the model scripts (for example ``red_pitaya_z20.xdc``, ``red_pitaya_4ADC.xdc``).

|

Constraint Selection Workflow
==============================

1. Choose your target ``MODEL``.
2. Identify the corresponding base ``sdc/<...>.xdc`` file from the mapping table above.
3. Add a project-level ``prj/<PRJ>/sdc/<same_name>.xdc`` only if you need project-specific changes for that model.
4. Build with your intended flags:

   .. code-block:: bash

      make PRJ=<your_project> MODEL=<model>

5. Review Vivado messages for:

   - unconstrained ports
   - conflicting constraints
   - I/O standard conflicts
   - timing violations

.. note::

    Keep project constraints minimal and explicit. Prefer inheriting common constraints from root ``sdc/`` and overriding only what is necessary in ``prj/<PRJ>/sdc/``.

|

Verification Checklist
=======================

Before hardware testing:

- Confirm ``MODEL`` matches the actual board.
- Confirm selected root ``.xdc`` matches ``MODEL``.
- Confirm any project override file name matches the expected model-specific name.
- Verify all external connector pins used by your design are constrained.
- Verify I/O standards and clock constraints for modified interfaces.

|

Related Pages
==============

- :ref:`fpga_create_project` - Standard scripted project creation flow
- :ref:`fpga_projects` - Repository and project overview
- :ref:`signal_mapping` - Physical signal and connector mapping
- :ref:`fpga_advanced_loading` - Runtime loading and overlay workflows
