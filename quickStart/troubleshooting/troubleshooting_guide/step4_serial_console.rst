.. _troubleshooting_serial_console:

#######################################
Step 4: Serial console boot log
#######################################

Checking the serial console boot log is an important step in troubleshooting network connection issues with Red Pitaya. By connecting to the Red Pitaya board 
over a serial console, we can inspect the boot messages for any errors that may be preventing the board from connecting to the network. Additionally, this is 
also the final check to determine if the Red Pitaya board is functioning properly. If the board is not booting correctly, it may indicate a hardware issue 
that requires further investigation.

Either way, the main goal is to get the serial console boot log and check for any errors during the boot process.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Prerequisites
================

Checking the serial boot log is necessary in the following scenarios:

* **Status LEDs are not working properly**. If the status LEDs are not working properly, this indicates that there is a problem with the Red Pitaya board itself
  (as a follow up to the :ref:`Step 2: Check the status LEDs <troubleshooting_status_leds>` chapter).
* **Status LEDs are working normally** and we are having **trouble connecting to the board**. This indicates that the Red Pitaya board is good, but we are still having 
  trouble connecting to the Red Pitaya web interface or :ref:`SSH <ssh>`.

|

Step-by-step serial troubleshooting
=====================================

The only option now is to connect to the Red Pitaya board over a :ref:`serial console <console>` and check the boot log for any errors. Please follow the steps below:

Establish a serial console connection
--------------------------------------

1. **Power-up the board**. Power up the board and connect the Ethernet cable as normal.
#. **Connect serial console**. After booting the board, connect a :ref:`serial console <console>` cable between the Red Pitaya board and your computer. Follow the instructions 
   in the :ref:`serial console connection guide <console>` to establish a connection. After establishing the connection, return back to this section and follow the next steps.

|

Boot log inspection
---------------------

3. **Boot log present**. Red Pitaya should print information about the boot sequence. If there is no feedback, there is a high chance the Zynq SoC is damaged. 
   Please :ref:`contact us <report_problem>`.
#. **U-Boot booting**. Check that the Zynq SoC (U-Boot) is booting (message *Autoboot will start in 3...2...1... (Hit any key to stop)*).
#. **Linux kernel booting**. On OS 2.07 and older, check that the Linux kernel boot sequence shows no :ref:`signs of looping <faq_rebooting>`. If the board is rebooting, 
   please check if you have an **External Clock board and the external clock is connected** and has the correct :ref:`specifications <faq_clock_specifications>`.
#. **Ubuntu welcome message reached**. If the kernel boot reaches the Linux Ubuntu welcome message and does not reboot, then the Red Pitaya hardware is fine.

|

After the Ubuntu welcome message
---------------------------------

If the board reaches the Ubuntu welcome message and does not reboot, then the hardware is most likely fine. If connection is still not possible, please check the 
following:

7. **Hostname check (rp-888888)**. If the boot log shows that the board is booting with the name ``rp-888888``, this means that the board may not be able to read the 
   MAC address from the EEPROM. Please install the :ref:`latest OS version <prepareSD>` and check if the issue persists. If it does, please :ref:`contact us <report_problem>`.
#. **OpenBSD failed to start error**. If the boot log shows the message **OpenBSD failed to start**, please wait one or two minutes, as the board may retry starting OpenBSD.
#. **Restart router and reboot board**. If the OpenBSD issue persists, restart the router (or clear the internal cache/ARP table of the router) and reboot the board, 
   then try connecting to the board again.
#. **Ping the Red Pitaya while serial console is open**. Use the ``ping`` command with both the Red Pitaya IP address and ``rp-xxxxxx.local``.
#. **Check IP assignment**. In the serial console, verify that the Ethernet interface received an IP address. If no IP is assigned, check the DHCP settings on the router.
#. **Check local network restrictions**. Some networks block device-to-device traffic (for example guest networks, AP/client isolation, or managed networks). If possible, 
   test on a different local network.

|

Next steps
============

If you have completed the Serial console check and the issue persists, please proceed to the next troubleshooting step: 

* :ref:`Step 7: Advanced troubleshooting <troubleshooting_advanced>`.
