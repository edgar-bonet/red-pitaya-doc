.. _troubleshooting_status_leds:

#####################
Step 2: Status LEDs
#####################

In this section we will check the status LEDs on the Red Pitaya board to determine if there are any hardware issues. The status 
LEDs provide valuable information about the state of the board and can help identify potential problems.

.. figure:: ../img/blinking-pitaya-eth.gif
    :align: center
    :width: 600

Red Pitaya Status LED Description:

    * :green:`Green LED` - Power good.
    * :blue:`Blue LED` - FPGA image loaded and OS booted.
    * :red:`Red LED` - CPU heartbeat.
    * :orange:`Orange LED` - SD card access.

|

Step-by-step LED check
=======================

Follow the steps below and stop when your board matches one of the LED patterns.

1. Check the Green LED first
----------------------------------

The :green:`Green LED` is the most important one of all the status LEDs, as it indicates whether the Red Pitaya board is receiving power properly.

If the :green:`Green LED` is **OFF** or **blinking**, this points to a power issue.

1.  **Power supply functionality** - Confirm the power supply is plugged into the ``PWR`` USB port.
2.  **Cable issue** - Reseat the cable (unplug and plug it in again), or try a different cable.
3.  **Verify power delivery**:

    * **Gen 2** boards require **5V / 3A**
    * **Original Gen** boards require **5V / 2A**
    * **SIGNALlab 250-12** requires **24V / 0.5A**

    Computer USB ports typically provide only 0.5A, which is not enough.

4.  **Issue remains** - If the issue remains, this indicates a hardware problem with the power supply. Please :ref:`contact us <report_problem>`.

|

2. Green ON, Blue OFF, Orange barely lit or OFF
-----------------------------------------------------

This usually indicates a problem loading the Red Pitaya file system from the SD card.

1.  **Power supply functionality** - Confirm the power supply is plugged into the ``PWR`` USB port.
2.  **Micro SD card** - Confirm the micro SD card is inserted.
3.  **Red Pitaya OS** - Confirm that Red Pitaya OS is installed on the SD card.

    * If needed, :ref:`reinstall the Red Pitaya OS on the SD card <prepareSD>`.
    * SD cards that ship with Red Pitaya should already include the official OS, but occasionally they may be empty or outdated.

4.  **3rd-party OS** - If you are using a 3rd-party OS (for example **Pavel Demin's Alpine Linux**), this can be normal behavior.

    * In some 3rd-party images, status LEDs are normally off.
    * Check the |red_pitaya_notes| or the relevant 3rd-party documentation.

5.  **EEPROM reading issue** - Check for an incorrect ``hw_rev`` value or EEPROM reading issue.

    * OS versions 2.00 and higher require a correct ``hw_rev`` in EEPROM.
    * If the issue started after an OS update, verify ``hw_rev`` using GitHub issue |#250|.
    * The RMA terms in that issue apply regardless of warranty status.

6.  **SD card corruption** - Check for SD card corruption.

    * Test the SD card in another Red Pitaya board, or
    * Replace the SD card and install a fresh OS image.

7.  **Inspect SD card holder** - Check for bent pins in the SD card holder. If any are bent upwards and are not in contact with the 
    pins of the SD card, remove the SD card and push them into the normal position with a toothpick or similar tool.

8.  **Issue remains** - If still unresolved, this likely indicates a hardware problem. Skip straight to 
    :ref:`Step 4: Serial console boot log <troubleshooting_serial_console>` and inspect boot messages for SD or file-system errors.

|

3. Green and Blue ON, Red and Orange cycle repeatedly
-----------------------------------------------------------

Pattern:

* :green:`Green` and :blue:`Blue` LEDs turn ON.
* :red:`Red` and :orange:`Orange` LEDs flash briefly.
* :red:`Red` and :orange:`Orange` LEDs then either stay ON or turn OFF both for about 2 seconds.
* The same cycle repeats.

This indicates a reboot loop.

1. **Verify external clock connection** - If you have an External Clock version of a Red Pitaya board, verify that the external clock 
   is connected to :ref:`E2 <E2_orig_gen>`.
2. **Verify external clock speed** - Verify the input clock matches :ref:`recommended clock specifications <faq_clock_specifications>`.
3. **Issue remains** - If the clock connection/specification is correct and the reboot loop continues, please :ref:`contact us <report_problem>`.
4. **Using a normal board?** - If you are using a normal Red Pitaya board (not an External Clock version), this may indicate a hardware problem. 
   Please :ref:`contact us <report_problem>`.

.. note::

    With OS 3.00 and higher, the external clock Red Pitaya boards will boot even without an external clock connected. However, 
    the board will not operate properly without a clock.

|

4. Green ON, Blue OFF, Red heartbeat, Orange activity during boot
----------------------------------------------------------------------

Pattern:

* :green:`Green` LED is ON.
* :blue:`Blue` LED is OFF.
* :red:`Red` LED flashes in a heartbeat pattern.
* :orange:`Orange` LED flashes sporadically during boot and usually turns OFF after about 1 minute.

This behaviour is quite close to the normal boot pattern, but the :blue:`Blue` LED should be ON after boot.

1. **Damaged blue LED** - Check that the :blue:`Blue` LED is not damaged. If the board is under warranty, we will replace it.

|

5. Green and Blue ON, Red heartbeat, Orange activity during boot
----------------------------------------------------------------------

Pattern:

* :green:`Green` and :blue:`Blue` LEDs are ON.
* :red:`Red` LED flashes in a heartbeat pattern.
* :orange:`Orange` LED flashes sporadically during boot and usually turns OFF after about 1 minute.

This is normal behavior and indicates the board is booting properly and the hardware is functioning correctly.

|

Next steps
============

If you have completed the LED check and the issue persists, please proceed to the next troubleshooting step: 

* Cannot connect to the web interface? Follow :ref:`Step 3: Network connection <troubleshooting_network>`. Or click **Next** in the bottom right corner 
  of this page (both lead to the same page).
* Issue with web interface or signals? Follow :ref:`Step 5: Hardware connections <troubleshooting_hardware_connections>`.



.. substitutions

.. |red_pitaya_notes| replace:: :github:`Pavel Demin's Red Pitaya Notes <pavel-demin/red-pitaya-notes>`
.. |#250| replace:: :rp-github:`#250 <RedPitaya/issues/250>`
.. |#254| replace:: :rp-github:`#254 <RedPitaya/issues/254>`