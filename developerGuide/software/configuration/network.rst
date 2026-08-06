.. _network:

#######
Network
#######

This section contains detailed information about the network configuration on Red Pitaya. It is intended for advanced users and developers who 
want to understand the details of the network configuration. For most users the :ref:`Network manager application <network_manager>` is the preferred 
way to configure the network.

.. contents:: Table of Contents
   :local:
   :depth: 2

|

Quick setup
==============

Red Pitaya supports two wireless modes:

* **Client mode** - create ``/opt/redpitaya/wpa_supplicant.conf``.
* **Access point mode** - create ``/opt/redpitaya/hostapd.conf`` and remove ``/opt/redpitaya/wpa_supplicant.conf``.

.. note::

	A reboot is not required to switch between access point and client modes. The Network Manager scripts restart the relevant services after 
	updating the configuration files.

|

Network configuration
----------------------

The current network configuration is using `systemd-networkd <https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html>`_ 
as the base. Almost all network configuration details are done by the bash script  :rp-github:`network.sh <ubuntu/blob/main/debian/network.sh>` 
during the creation of the Debian/Ubuntu SD card image. The script installs networking related packages and copies network configuration files 
from the Git repository.

The decision to focus on ``systemd-networkd`` is arbitrary, while at the same time focusing at a single approach centered around 
`systemd <https://systemd.io/>`__ should minimize the efforts needed to maintain it.

Most of the WiFi configuration complexity comes from support for switching between WiFi access point and client mode.

|

UDEV
====

``systemd`` provides `predictable network interface names <https://www.freedesktop.org/wiki/Software/systemd/PredictableNetworkInterfaceNames/>`_ 
using `UDEV <https://www.freedesktop.org/software/systemd/man/latest/udev.html>`__ rules. In our case the kernel names the USB WiFi 
adapter ``wlan0``, then ``UDEV`` rule ``/lib/udev/rules.d/73-usb-net-by-mac.rules`` renames it into ``enx{MAC}`` using the following rule:

.. code-block:: shell-session

	# Use MAC based names for network interfaces which are directly or indirectly
	# on USB and have an universally administered (stable) MAC address (second bit
	# is 0).
	
	IMPORT{cmdline}="net.ifnames", ENV{net.ifnames}=="0", GOTO="usb_net_by_mac_end"
	PROGRAM="/bin/readlink /etc/udev/rules.d/80-net-setup-link.rules", RESULT=="/dev/null", GOTO="usb_net_by_mac_end"
	
	ACTION=="add", SUBSYSTEM=="net", SUBSYSTEMS=="usb", NAME=="", \
		ATTR{address}=="?[014589cd]:*", \
		IMPORT{builtin}="net_id", NAME="$env{ID_NET_NAME_MAC}"
	
	LABEL="usb_net_by_mac_end"


For a simple generic WiFi configuration it is preferred to have the same interface name regardless of the used adapter. 
This is achieved by overriding ``UDEV`` rules with a modified rule file. The overriding is done by placing the modified 
rule file into directory ``/etc/udev/rules.d/73-usb-net-by-mac.rules``. Since the remaining rules in the file are not 
relevant on Red Pitaya, it is also possible to deactivate the rule by creating a override file which links to ``/dev/null``.

.. code-block:: shell-session

   	# ln -s /dev/null /etc/udev/rules.d/73-usb-net-by-mac.rules

|

Wired setup
===========

The wired interface ``eth0`` configuration file :rp-github:`/etc/systemd/network/wired.network <ubuntu/blob/main/debian/overlay/etc/systemd/network/wired.network>`
configures it to use DHCP.

In the pre-1.04 OS releases, where a `different DHCP client was used <https://linux.die.net/man/8/dhclient>`__, it was possible to define a fixed lease, 
which would provide a fallback address if DHCP fails. Using the ``systemd`` integrated DHCP client this is not possible, instead a fixed address can be set, 
or Link-Local addressing (zeroconf) can be used (described later).

A static IP address can be chosen by modifying the configuration file. It is also possible to have both a DHCP provided and a static address 
at the same time, but this is not a good choice for the release default since it can cause IP address collisions. A fixed IP address can be 
configured by adding the following lines to `systemd-networkd <https://www.freedesktop.org/software/systemd/man/latest/systemd.network.html>`_ files.

.. code-block:: none

	[Network]
	Address=192.168.0.15/24
	Gateway=192.168.0.1

|

WiFi client
===========

A list of :ref:`Supported USB Wi-Fi adapters <support_wifi_adapter>` is provided at the bottom of the page.

List wireless access points:

.. code-block:: shell-session

   	# iw wlan0 scan | grep SSID


Write a ``wpa_supplicant.conf`` configuration file to the FAT partition. ``ssid`` and ``passphrase`` can be provided in angle brackets.

.. code-block:: shell-session

	# rw
	$ wpa_passphrase <ssid> [passphrase] > /opt/redpitaya/wpa_supplicant.conf


Restart WPA supplicant:

.. code-block:: shell-session

    # systemctl restart wpa_supplicant@wlan0.service

|

WiFi access point
=================

Write a `hostapd.conf <https://git.w1.fi/cgit/hostap/plain/hostapd/hostapd.conf>`_ configuration file to the FAT partition,
and remove the ``wpa_supplicant.conf`` client configuration file if it exists:

.. code-block:: shell-session

	# rw
	$ nano /opt/redpitaya/hostapd.conf
	$ rm /opt/redpitaya/wpa_supplicant.conf


Restart access point service:

.. code-block:: shell-session

   	# systemctl restart hostapd@wlan0.service

|

.. _wireless_setup:

Wireless setup
==============

The wireless interface ``wlan0`` configuration file is :rp-github:`/etc/systemd/network/wireless.network <ubuntu/tree/main/debian/overlay/etc/systemd/network>`.

To support two modes this file must be linked to either the client mode configuration
:rp-github:`/etc/systemd/network/wireless.network.client <ubuntu/blob/main/debian/overlay/etc/systemd/network/wireless.network.client>`
or the access point configuration
:rp-github:`/etc/systemd/network/wireless.network.ap <ubuntu/blob/main/debian/overlay/etc/systemd/network/wireless.network.ap>`.
Switching between the two options is implemented by
:rp-github:`/etc/systemd/system/wireless-mode-ap.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/wireless-mode-ap.service>`
and
:rp-github:`/etc/systemd/system/wireless-mode-client.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/wireless-mode-client.service>`
which must be run early at boot before most other network related services are run. If no wireless configuration file is available, then a third service
:rp-github:`/etc/systemd/system/wireless_adapter_up@.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/wireless_adapter_up@.service>`
will link ``wireless.network`` to client mode, and it will power up the adapter so that ``iwlist`` will work.

The choice of the interface is driven by the availability of access point ``/opt/redpitaya/hostapd.conf`` and client ``/opt/redpitaya/wpa_supplicant.conf`` 
configuration files. If ``wpa_supplicant.conf`` is present, client mode configuration will be attempted, regardless of the presence of ``hostapd.conf``.
If only ``hostapd.conf`` is present access point configuration will be attempted. If no configuration file is present, WiFi will not be configured.

+----------------------------+------------------------------+
| File                       | Comment                      |
+============================+==============================+
| ``wpa_supplicant.conf``    | Client configuration         |
+----------------------------+------------------------------+
| ``hostapd.conf``           | Access point configuration   |
+----------------------------+------------------------------+

|

Wireless client setup
---------------------

Wireless networks almost universally use some kind of encryption/authentication scheme for security. This is handled by the tool `wpa_supplicant <https://w1.fi/wpa_supplicant/>`_.
The default network configuration option on `Debian Network Manager <https://wiki.debian.org/NetworkManager>`_ / 
`Ubuntu Network Manager <https://help.ubuntu.com/community/NetworkManager>`_ is `Network Manager <https://wiki.gnome.org/Projects/NetworkManager>`_. Sometimes 
it conflicts with the default ``systemd-networkd`` install, this seems to be one of those cases. On `Debian <https://packages.debian.org/trixie/wpasupplicant>`_
/ Ubuntu a device `specific @.service <https://git.w1.fi/cgit/hostap/tree/wpa_supplicant/systemd/wpa_supplicant.service.arg.in>`_ service is missing, so we 
made a copy :rp-github:`copy of wpa_supplicant@.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/wpa_supplicant%40.service>` in our Git repository.

By default the service is installed as a dependency for ``multi-user.target`` which means it would delay ``multi-user.target`` if it could not start 
properly, for example due to the USB WiFi adapter not being plugged in. At the same time the service was not automatically started after the adapter 
was plugged into Red Pitaya. The next change fixes both.

.. code-block:: shell-session

	[Install]
	-Alias=multi-user.target.wants/wpa_supplicant@%i.service
	+WantedBy=sys-subsystem-net-devices-%i.device


The encryption/authentication configuration file is linked to the FAT partition for easier user access. So it is enough to provide a proper 
``wpa_supplicant.conf`` file on the FAT partition to enable wireless client mode.

.. code-block:: shell-session

   	# ln -s /opt/redpitaya/wpa_supplicant.conf /etc/wpa_supplicant/wpa_supplicant.conf


This configuration file can be created using the ``wpa_passphrase`` tool:

.. code-block:: shell-session

   	$ wpa_passphrase <ssid> [passphrase] > /opt/redpitaya/wpa_supplicant.conf

|

Wireless access point setup
---------------------------

WiFi access point functionality is provided by the `hostapd <https://w1.fi/hostapd/>`__ application. Since the upstream version does not support the 
``wireless extensions`` API, the application is not installed as a Debian package, and is instead downloaded, patched, recompiled and installed.

The :rp-github:`hostapd@.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/hostapd%40.service>` is handling the start of the daemon. 
Hotplugging is achieved the same way as with ``wpa_supplicant@.service``.

To enable access point mode a `hostapd.conf <https://git.w1.fi/cgit/hostap/plain/hostapd/hostapd.conf>`_ configuration file must be placed on the FAT 
partition on the SD card, and the client mode configuration file ``wpa_supplicant.conf`` must be removed. Inside a shell on Red Pitaya this file is 
visible as ``/opt/redpitaya/hostapd.conf``.

.. code-block:: none

	interface=wlan0
	ssid=<ssid>
	driver=nl80211
	hw_mode=g
	channel=6
	macaddr_acl=0
	auth_algs=1
	ignore_broadcast_ssid=0
	wpa=2
	wpa_passphrase=<passphrase>
	wpa_key_mgmt=WPA-PSK
	wpa_pairwise=TKIP
	rsn_pairwise=CCMP


This file must be edited to set the chosen ``<ssid>`` and ``<passphrase>``. Other settings are for the currently most secure personal encryption.

|

Wireless router
~~~~~~~~~~~~~~~

In access point mode Red Pitaya behaves as a wireless router, if the wired interface is connected to the local network.

In the wired network configuration file :rp-github:`/etc/systemd/network/wired.network <ubuntu/blob/main/debian/overlay/etc/systemd/network/wired.network>`
there are two lines to enable IP forwarding and masquerading.

.. code-block:: none

	IPForward=yes
	IPMasquerade=yes


An iptables configuration :rp-github:`/etc/iptables/iptables.rules <ubuntu/blob/main/debian/overlay/etc/iptables/iptables.rules>` is enabled by 
the iptables service :rp-github:`/etc/systemd/system/iptables.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/iptables.service>`.

.. warning::
	
	**SECURITY WARNING:** This functionality combined with default passwords can be a serious security issue and since it is not needed for 
	normal Red Pitaya functionality, it is currently disabled.

	Users are free to enable it, but they should be aware of the security implications.

|

``systemd`` services
====================

Services handling the described configuration are enabled with.

.. code-block:: shell-session

	# enable systemd network related services
	systemctl enable systemd-networkd
	systemctl enable systemd-resolved
	systemctl enable systemd-timesyncd
	systemctl enable wpa_supplicant@wlan0.service
	systemctl enable hostapd@wlan0.service
	systemctl enable wireless-mode-client.service
	systemctl enable wireless-mode-ap.service
	systemctl enable iptables.service
	#systemctl enable wpa_supplicant@wlan0.path
	#systemctl enable hostapd@wlan0.path
	systemctl enable hostname-mac.service
	systemctl enable avahi-daemon.service
	
	# enable service for creating SSH keys on first boot
	systemctl enable ssh-reconfigure

|

DNS Resolver
============

To enable the ``systemd`` integrated resolver, a symlink for ``/etc/resolv.conf`` must be created.

.. code-block:: shell-session

   	# ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf


It is also possible to add default DNS servers by adding them to ``*.network`` files.

.. code-block:: none

	nameserver=8.8.8.8
	nameserver=8.8.4.4

|

NTP (Network Time Protocol)
===========================

Instead of using the common ``ntpd`` the lightweight ``systemd-timesyncd`` `SNTP <https://www.ntp.org/ntpfaq/NTP-s-def/#AEN1271>`_ client is used. y
Since by default NTP servers are provided by DHCP, no additional configuration changes to `timesyncd.conf <https://www.freedesktop.org/software/systemd/man/latest/timesyncd.conf.html>`_ 
are needed.

To observe the status of time synchronization, run:

.. code-block:: shell-session

   	$ timedatectl status


To enable the service, run:

.. code-block:: shell-session

   	# timedatectl set-ntp true

|

SSH server
==========

The OpenSSH server is installed and access to the root user is enabled.

At the end of the SD card Debian/Ubuntu image creation encryption certificates are removed. They are again created on the first boot by 
:rp-github:`/etc/systemd/system/ssh-reconfigure.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/ssh-reconfigure.service>`.
Due to this the first boot takes a bit longer. This way the SSH encryption certificates are unique on each board.

|

Zero-configuration networking
=============================

Link-local address
------------------

``systemd-networkd`` can provide interfaces with `link-local addresses <https://en.wikipedia.org/wiki/Link-local_address>`_, if this is enabled inside 
``systemd.network`` files with the line ``LinkLocalAddressing=yes``. All interfaces have this setting enabled, this way each active interface will acquire 
an address in the reserved ``169.254.0.0/16`` address block.

|

Zeroconf
--------

If the computer used to access the device supports zeroconf (Avahi/Bonjour) name resolving is also available. Since there can be multiple 
devices on a single network they must be distinguished. The last three segments of the Ethernet MAC number without semicolons (as printed 
on the Ethernet connector on each device) is used to generate the hostname, which is then used to generate a link name. For example if the 
MAC address is ``00:26:32:f0:f1:f2`` then the shortened string ``shortMAC`` is ``f0f1f2``.

Hostname generation is done by :rp-github:`/etc/systemd/system/hostname-mac.service <ubuntu/blob/main/debian/overlay/etc/systemd/system/hostname-mac.service>`
which must run early during the boot process. To set your own hostname, replace the following line in ``hostname-mac.service``:

.. code-block:: shell-session

	hostnamectl set-hostname / * MY HOST NAME * /


Each device can now be accessed at ``http://rp-<shortMAC>.local``.

Similarly to get SSH access use.

.. code-block:: shell-session

   	$ ssh root@rp-<shortMAC>.local


This service is a good alternative for our *Discovery* service provided on redpitaya.com servers.

`Avahi daemon <https://avahi.org/>`_ is used to advertise specific services. Three configuration files are provided.

* HTTP :rp-github:`/etc/avahi/services/bazaar.service <ubuntu/blob/main/debian/overlay/etc/avahi/services/bazaar.service>`
* SSH  :rp-github:`/etc/avahi/services/ssh.service    <ubuntu/blob/main/debian/overlay/etc/avahi/services/ssh.service>`
* SCPI :rp-github:`/etc/avahi/services/scpi.service   <ubuntu/blob/main/debian/overlay/etc/avahi/services/scpi.service>`

|

.. _support_wifi_adapter:

WiFi Adapter Compatibility
==========================

Support for a specific USB Wi-Fi adapter depends primarily on the Linux driver support for its chipset.

Compatibility by OS version
---------------------------

* **OS 3.00 and higher** - RTL8812BU and RTL8188CUS based adapters are supported. Access point mode is disabled by default.
* **OS 2.00 and higher** - RTL8192CU and RTL8188CUS class adapters are supported in client mode.
* **OS 1.04 and older** - BCM43143 supports client and access point mode; RTL8192CU class adapters are client-mode only.

Recommended adapters
--------------------

* **OS 3.00 and higher** - TP-Link Archer T3U AC1300 (RTL8812BU).
* **OS 2.00 and older** - :rp-web:`Red Pitaya WiFi Dongle <product/red-pitaya-wi-fi-dongle>` or Edimax EW-7811Un V2.

.. note::

	The list of supported adapters is not exhaustive. Other adapters may work, but they are not officially supported. The main requirement is that the 
	adapter's chipset must be supported by the Linux kernel used in the Red Pitaya OS. Other chipsets can be manually added to the kernel.

Chipset summary
---------------

+-----------------------------+-----------------+-----------------------+---------------------------------------------+
| Chipset                     | Client mode     | Access point mode     | Notes                                       |
+=============================+=================+=======================+=============================================+
| RTL8812BU                   | Yes             | Disabled by default   | Main target for OS 3.00 and higher.         |
+-----------------------------+-----------------+-----------------------+---------------------------------------------+
| RTL8192CU / RTL8188CUS      | Yes             | Disabled              | Uses ``rtl8xxxu`` class drivers in practice.|
+-----------------------------+-----------------+-----------------------+---------------------------------------------+
| BCM43143                    | Yes             | Disabled              | Mostly relevant for older OS releases.      |
+-----------------------------+-----------------+-----------------------+---------------------------------------------+

.. note::

	The table shows the status for the most recent OS version. Older OS releases may have different support for the same chipset (see the 
	`Compatibility by OS version`_ section above).


How to verify adapter support
-----------------------------

After plugging in a USB Wi-Fi adapter, use these commands:

.. code-block:: shell-session

	$ lsusb
	$ dmesg | tail -n 50
	$ iw list

Check whether your adapter is detected and whether ``Supported interface modes`` includes the mode you need (``managed`` or ``AP``).

|
