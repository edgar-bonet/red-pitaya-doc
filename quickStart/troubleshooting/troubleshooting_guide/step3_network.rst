.. _troubleshooting_network:

###############################
Step 3: Network connection
###############################

In this section we will provide guidance on how to troubleshoot network connection issues that may arise when using Red Pitaya.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Prerequisites
================

At this stage the Red Pitaya board should be powered on and the status LEDs should be working normally. If the status LEDs are not working normally, 
please refer to the :ref:`Step 2: Check the status LEDs <troubleshooting_status_leds>` chapter.

|

Step-by-step network troubleshooting
=====================================

Before proceeding with the steps below, please check if you can :ref:`connect to the board via the web interface <quickstart_connect>`. If you cannot connect to the web interface, please 
follow the steps below to troubleshoot the network connection.

Check software
---------------

1.  **Use updated Google Chrome**. Use the latest version of Google Chrome as your web browser. Some features of the web interface may not work properly on other browsers or older versions of Chrome.
#.  **Disable adblockers** for the ``rp-xxxxxx.local`` website.
#.  **Disable the VPN** as it may be preventing the connection.
#.  **Check firewall or antivirus software**. Some security tools may block the web interface, SSH, or local network discovery. Navigate to the **Network firewall** settings and check if Red Pitaya is 
    blocked (look for **Resolve blocked communication** or similar options). If the board is blocked, allow it to communicate through the firewall.
#.  **Clear the browser cache** if the page opens only partly or shows old information.

|

Check hardware
-----------------

6.  **Verify cable connections**. Consult the :ref:`connection guide <quickstart_connect>` for advice.
#.  **Check the Ethernet cable and socket for damage**. Plug the Red Pitaya Ethernet cable into the computer and verify internet connection. Replace the Ethernet cable and/or try a different 
    Ethernet socket on the router.

|

Check network basics
---------------------

8.  **Same local network**. Make sure your Red Pitaya and computer are both connected to the same :ref:`local network <faq_connected>`. 
#.  **Complex networks**.In complex networks (multiple routers/switches/access points), verify both devices are on the same subnet.
#.  **Check network listing**. Use the ``arp -a`` command in ``Command Prompt`` or ``Terminal`` to locate Red Pitaya's MAC and IP address.
#.  **Ping the Red Pitaya**. Use the ``ping`` command with the Red Pitaya IP address and with the ``rp-xxxxxx.local`` address.
#.  **Use IP address instead of .local**. Enter the IP address in the browser URL instead of ``rp-xxxxxx.local``.
#.  **Guest networks and isolation**. Make sure the computer is not connected to a guest network, and check whether the router has client isolation or AP isolation enabled.
#.  **Check network security**. Some networks may have security restrictions that prevent you from connecting (for example, university networks require all devices to connect through 
    a special web page to confirm identity). A Red Pitaya board may not be able to connect to such networks. Try connecting to a different network (for example, a home network) and see if the issue persists.
#.  **Connect the board to a router** instead of directly to the computer, then retry the steps above. The computer should be connected to the same router either via Ethernet or Wi-Fi.

|

Check router settings
-----------------------

16.  **Confirm that DHCP is enabled** on your router.
#.  **Restart router or clear the router's internal cache/ARP table**. Router has its own internal cache/ARP table, which stores client information. Each entry has a specific duration (usually an hour or more), 
    so if the client information is outdated, it may cause connectivity issues. For Red Pitaya boards this is especially important, since OS can be updated on the fly, which can cause connection issues 
    unrelated to the board. If Red Pitaya OS is updated, the router may still have the old MAC address in its cache, which can cause connectivity issues. Restarting the router or clearing the router's 
    internal cache/ARP table can resolve this issue.
#.  **Check for duplicate IP addresses**. If another device already uses the same IP address, the connection may fail or become unstable.
#.  **Check static IP settings**. If the Red Pitaya or the computer uses a manual IP address, make sure it is in the same address range as the rest of the network.
#.  **Use a different computer and router**. Some networks may have security restrictions that prevent you from connecting (for example, university networks require all devices to connect through 
    a special web page to confirm identity).

|

Computer OS settings
-----------------------

It is quite common for computer OS settings to prevent the connection to Red Pitaya. Please check the following:

21. **Check network privacy settings**. Make sure the network is set to **Private** (Windows) or **Trusted** (Linux and MacOS). If the network is set to **Public** or **Untrusted**, the computer may 
    block the connection to Red Pitaya. This is especially common with **direct connections** (Red Pitaya connected directly to the computer via Ethernet cable).

    You may need to adjust these settings via command line (admin access may be required). Here is an example for Windows 11:

    .. code-block:: powershell

        # Check the network profile
        Get-NetConnectionProfile

        # Change the network profile to Private
        Set-NetConnectionProfile -Name "NetworkName" -NetworkCategory Private

    .. code-block:: powershell

        PS C:\Users\localadmin> Get-NetConnectionProfile

        Name                     : Unidentified network
        InterfaceAlias           : Ethernet
        InterfaceIndex           : 3
        NetworkCategory          : Public
        DomainAuthenticationKind : None
        IPv4Connectivity         : LocalNetwork
        IPv6Connectivity         : NoTraffic

        Name                     : WifiName
        InterfaceAlias           : WiFi
        InterfaceIndex           : 22
        NetworkCategory          : Public
        DomainAuthenticationKind : None
        IPv4Connectivity         : Internet
        IPv6Connectivity         : NoTraffic

        PS C:\Users\localadmin> Set-NetConnectionProfile -InterfaceIndex 3 -NetworkCategory Private
        PS C:\Users\localadmin> Get-NetConnectionProfile


        Name                     : Unidentified network
        InterfaceAlias           : Ethernet
        InterfaceIndex           : 3
        NetworkCategory          : Private
        DomainAuthenticationKind : None
        IPv4Connectivity         : LocalNetwork
        IPv6Connectivity         : NoTraffic

        Name                     : WifiName
        InterfaceAlias           : WiFi
        InterfaceIndex           : 22
        NetworkCategory          : Public
        DomainAuthenticationKind : None
        IPv4Connectivity         : Internet
        IPv6Connectivity         : NoTraffic

#.  **Verify Ethernet port settings (Linux and MacOS only)**. If you are a Linux or MacOS user and the Red Pitaya is connected directly to the computer (via the Ethernet cable), 
    check the Ethernet port IPv4 and IPv6 settings to see if they are set to **DHCP** and **Local Only**. Alternatively, connect to the Red Pitaya via a router.
#.  **Content and privacy settings (MacOS)**. If a Mac computer will not connect to the Red Pitaya, it is possible that **Content and privacy settings** are blocking websockets. 
    After updating the settings you will need to log out and log in again. It may be necessary to completely disable content and privacy settings.

    .. figure:: ../img/MAC_content_privacy.png
        :width: 800

    .. figure:: ../img/MAC_content_privacy2.png
        :width: 600

    .. figure:: ../img/MAC_content_privacy3.png
        :width: 600

#.  **Confirm mDNS and DNS-SD is available (older Windows only)**.

        * **Windows 10 or higher** already supports mDNS and DNS-SD, so there is no need to install any additional software.
        * **Windows 7/8** users should install :rp-download:`Bonjour Print Services <tools/BonjourPSSetup.exe>`, otherwise access to ``*.local`` addresses will not work.

#.  **Disable power saving on the network adapter**. Some computers put the Ethernet or Wi-Fi adapter to sleep, which can interrupt the connection.
        
|

Try isolation tests
-----------------------

26. **Try a different computer**. If possible, try connecting to the Red Pitaya board from a different computer on the same network.
#. **Try a different network**. If possible, try connecting to the Red Pitaya board from a different network (for example, a home network instead of a university network).

|

Next steps
============

If you have completed the Network check and the issue persists, please proceed to the next troubleshooting step: 

* :ref:`Step 4: Serial console boot log <troubleshooting_serial_console>`.
* Or click **Next** in the bottom right corner of this page (both lead to the same page).
