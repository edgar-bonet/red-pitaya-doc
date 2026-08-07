.. _troubleshooting_web_applications:

############################
Step 6: Web applications
############################

In this section we will check the most common issues with the web applications of the Red Pitaya board to determine if there are any problems with the 
web interface or the applications themselves.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Prerequisites
===============

Use this section if:

* **Hardware connections have been checked and are working properly**. If you have checked the :ref:`hardware connections <troubleshooting_hardware_connections>` and 
  they are working properly, but you are still experiencing issues with the web interface or the applications.
* **Web interface is accessible, but applications are not working properly**. If you are able to access the web interface, but the applications are not 
  working properly or are freezing.

|

Step-by-step web application troubleshooting
=============================================

The most likely cause is a problem with the web browser, the web applications, or the signal settings.

Check the browser
------------------

1. **Use an updated Google Chrome**. Make sure you are using the latest version of Google Chrome as your web browser. Some features of the web interface may not work properly on 
   other browsers or older versions of Chrome.
#. **Clear browser cache and cookies**. Clear the browser cache and cookies, then try accessing the web interface again.

|

Check the web applications
--------------------------

3. **Update the OS**. If the web interface is freezing or reloading, please :ref:`update the OS to the latest version <update_os>`. This was a common issue in older 
   Red Pitaya firmware (OS 1.04 and older) when using a direct connection. 
#. **Delete board application data**. Clear the application data on the Red Pitaya board by accessing the :ref:`System info menu <system_info>` and selecting **Delete application data**. 
   This will reset all application settings to default and should resolve issues caused by corrupted application data.

|

Check the signal settings
--------------------------

5. **Signals not showing in the web interface**. If you are able to access the web interface and the applications are working properly, but you are not receiving any signal on the 
   inputs or outputs of your Red Pitaya, please check the following:

    * **Show the signals**. Make sure the signals are shown in the web interface by confirming the **Show** button is highlighted in the input settings.
    * **Input jumpers**. Check the :ref:`input jumpers <jumper_pos>`. Sometimes the jumpers have poor contact and need to be removed and replaced. If the 
      jumpers are loose or missing, please replace them.
    * **Calibration settings**. A bad calibration can cause Red Pitaya to display incorrect measurements or even appear to detect no signal at all. This applies 
      to both the inputs and outputs of the Red Pitaya. Check the :ref:`DC calibration settings <calibration_app>` in the web interface and reset to **default** 
      and recalibrate the board if necessary. This will update the User calibration to the latest format and should fix problems with signals not showing 
      in the web interface.

|

Next steps
=============

If you have completed the Web applications check and the issue persists, please proceed to the next troubleshooting step:

* :ref:`Step 7: Advanced troubleshooting <troubleshooting_advanced>`.
* Or click **Next** in the bottom right corner of this page (both lead to the same page).

|
