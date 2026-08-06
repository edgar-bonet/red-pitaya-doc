
.. _troubleshooting_guide:

#########################
Troubleshooting guide
#########################

In this section we will provide some guidance on how to troubleshoot common issues that may arise when using Red Pitaya. If you are experiencing issues 
that are not covered in this guide, please refer to the :ref:`FAQ <faq>` section or contact our support team for assistance.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|


Normal operation
================

Before describing all the troubleshooting steps, let us first assert the base line of normal operation. The easiest way to split this is to divide 
the Red Pitaya operation into two parts:

* **The board itself** - The board is the physical device itself consisting of both hardware and software.
* **The network connection** - The network connection is the connection between the Red Pitaya and your computer, which consists of the network 
  cables, routers, switches, other network devices, and the network configuration on your computer.

Red Pitaya board
-----------------

The condition of Red Pitaya board can be checked through the status LEDs on the board. The following table describes the normal operation of 
the status LEDs:

+----------------------------+-----------------------+---------------------------------------+---------------------------------------------------------------+
| Colour                     | Function              | Blink pattern                         | Description                                                   |
+============================+=======================+=======================================+===============================================================+
| :blue:`Blue`               | FPGA bitstream status | Constantly ON                         | FPGA bitstream has been successfully loaded.                  |
+----------------------------+-----------------------+---------------------------------------+---------------------------------------------------------------+
| :green:`Green`             | Power supply status   | Constantly ON                         | All power supplies on Red Pitaya are working properly.        |
+----------------------------+-----------------------+---------------------------------------+---------------------------------------------------------------+
| :red:`Red`                 | CPU load status       | Heartbeat pattern                     | CPU is operating normally.                                    |
+----------------------------+-----------------------+---------------------------------------+---------------------------------------------------------------+
| :orange:`Orange`           | SD card access        | Sporadic flashing in slow intervals   | Blinking corresponds to SD card access.                       |
+----------------------------+-----------------------+---------------------------------------+---------------------------------------------------------------+

.. note::

    **E3 module** - :ref:`QSPI eMMC module <E3_QSPI_eMMC_module_HW>` rewires the orange LED to the internal watchdog timer (the LED will blink continuously).

|

Network connection
------------------

Network connection can be checked by connecting to the Red Pitaya board through the web interface or through SSH by using the .local address. 
If you are able to connect to the Red Pitaya board through either of these methods and the web interface loads, then the network connection 
is working properly.

|

Troubleshooting procedure
==========================

At this point, we have established the normal operation of the Red Pitaya board and the network connection. If you are experiencing issues with 
either of these, please follow the troubleshooting steps below.

The troubleshooting procedure is a step-by-step guide to troubleshooting common issues with Red Pitaya. It is recommended to follow the steps
in order, as they are designed to help you identify and solve the problem efficiently.

Each step focuses on a different part of the troubleshooting process:

* **Step 1: Update the OS/firmware** - update the Red Pitaya OS and firmware to the latest version.
* **Step 2: Status LEDs** - verify whether the board itself is operating normally.
* **Step 3: Network connection** - check local network access and connection problems.
* **Step 4: Serial console boot log** - inspect boot messages and identify startup errors.
* **Step 5: Hardware connections** - verify cables, external devices, and board connections.
* **Step 6: Web applications** - check the web interface and application behavior.
* **Step 7: Advanced troubleshooting** - review more specific hardware and software issues.

.. note::

    When troubleshooting a Red Pitaya board, **disconnect all devices and shields attached to the board**. This will help isolate the issue 
    and ensure that the problem is not caused by any external devices or shields.

    **Keep the external clock connected** - If you have an External Clock version of a Red Pitaya board, keep the external clock connected 
    during the troubleshooting process.

.. toctree::
    :maxdepth: 1

    troubleshooting_guide/step1_OS
    troubleshooting_guide/step2_LEDs
    troubleshooting_guide/step3_network
    troubleshooting_guide/step4_serial_console
    troubleshooting_guide/step5_hardware
    troubleshooting_guide/step6_web_apps
    troubleshooting_guide/step7_advanced



