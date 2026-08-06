.. _troubleshooting_hardware_connections:

#################################
Step 5: Hardware connections
#################################

In the hardware section we will check the hardware connections of the Red Pitaya board to determine if there are any issues with the signal reception or detection.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Prerequisites
===============

Use this section if:

* **Web interface is accessible, but no signal is received**. If you are able to access the web interface and the applications are working properly, but 
  you are not receiving any signal on the inputs or outputs of your Red Pitaya.
  
The most likely cause is a problem with hardware connections.

|

Step-by-step hardware troubleshooting
=====================================

#.  **Input jumpers**. Check the :ref:`input jumpers <jumper_pos>`. Sometimes the jumpers have poor contact and need to be removed and replaced. If the 
    jumpers are loose or missing, please replace them.
#.  **SMA cables**. Confirm that the SMA cables are not damaged, have bad contact, or are loose.
#.  **T-connectors and 50-Ω terminators**. If you are using T-connectors and 50-Ω terminators, check that they are properly connected and not damaged.
  
    .. note::

        SMA cables, T-connectors, terminators and other cables are consumables and may wear out over time. If you are experiencing issues with signal reception, 
        please test them or replace them with new ones.

#.  **Other cables and connections**. Confirm signal connections and power of the system. A misalignment in a laser system lens, an improperly connected probe, 
    or an unpowered component can sometimes cause the signal to not be received.
#.  **Calibration settings**. A bad calibration can cause Red Pitaya to display incorrect measurements or even appear to detect no signal at all. This applies 
    to both the inputs and outputs of the Red Pitaya. Check the :ref:`calibration settings <calibration_app>` in the web interface and reset to **default** 
    and recalibrate the board if necessary. This will update the User calibration to the latest format and should fix problems with signals not showing 
    in the web interface.

|

Next steps
=============

If you have completed the Hardware connections check and the issue persists, please proceed to the next troubleshooting step: 

* :ref:`Step 6: Web applications <troubleshooting_web_applications>`.
* Or click **Next** in the bottom right corner of this page (both lead to the same page).

|
