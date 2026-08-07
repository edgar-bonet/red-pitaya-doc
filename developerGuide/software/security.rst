.. _sw_security_considerations:

#######################
Security Considerations
#######################

This page summarizes how security works in practice on Red Pitaya and what users should consider when deploying systems in controlled or exposed environments.

Red Pitaya is designed primarily for open development, hardware access, and fast prototyping. Ease of use and developer flexibility are prioritized by default.

.. note::

   This page provides practical guidance, not a complete security standard for every deployment. If your device is used in a production, regulated, or internet-facing 
   environment, validate your own security controls and operating procedures.

| 

General principles
==================

* Red Pitaya boards are Linux computers and can be hardened according to your policy.
* Default behavior favors development access, not locked-down operation.
* Keep the OS and board software updated to receive Linux and ecosystem fixes.
* Never hardcode passwords, API keys, or tokens in source code.
* If your deployment requires stricter controls, apply additional user, service, and network restrictions.

| 

Quick hardening checklist
=========================

1. Change default credentials and use strong unique passwords.
2. Prefer SSH keys over password authentication where possible.
3. Restrict SSH and web access to trusted networks or VPN paths.
4. Disable or remove services that are not needed in your deployment.
5. Keep the OS and software stack updated, then re-test your application workflow.
6. Back up your configuration before applying major security policy changes.

| 

Access model and root privileges
================================

Many Red Pitaya applications control FPGA registers through memory-mapped interfaces. In the default Red Pitaya software stack, this typically means elevated or privileged 
access to low-level system resources.

**Key implications:**

* Web applications that configure acquisition, generation, or other FPGA-connected features depend on privileged hardware access.
* Creating a secondary user does not always provide strong privilege separation for these workloads, because the application path still needs access to memory-mapped FPGA control.
* Disabling default root access without planning alternative privilege paths can break expected functionality.

For this reason, Red Pitaya does not enforce strict user restrictions by default. Users who need stricter security should treat board hardening as a deployment task and 
customize Linux permissions, services, and network exposure to match their environment.

Possible architecture for stricter environments:

* Run only the minimum hardware-control components with elevated privileges.
* Keep user-facing services as unprivileged processes when feasible.
* Verify all affected applications after changing privilege boundaries.

| 

Users and authentication
========================

* Use strong passwords and rotate them for deployed systems.
* Decide your access model first: development convenience versus strict separation.
* If you disable direct root login, verify that all required applications and services still work through your chosen privilege model.
* Prefer restricted network exposure for production-like setups (trusted subnets, VPN, or firewall rules).

.. note::

   Security behavior can differ between OS releases, board models, and application stacks. Re-validate authentication and privilege assumptions after upgrades.

| 

Hardware access and permissions
===============================

When granting access to devices (GPIO, SPI, I2C, UIO, IIO, UART), avoid broad permissions where possible and assign only what is required.

Recommended approach:

1. Create dedicated groups for hardware-facing applications.
2. Apply udev rules that map specific device nodes to those groups.
3. Run the application process under that group if the access path supports non-root operation.

.. note::

   Some FPGA control paths rely on privileged memory-mapped access. In those cases, group-only device permissions may not be sufficient to fully replace root-based execution.

Useful commands for troubleshooting udev matching:

.. code-block:: shell-session

   udevadm info -a /dev/xdevcfg
   udevadm info -a /dev/uio0
   udevadm info -a /dev/spidev1.0
   udevadm info -a /dev/i2c-0
   udevadm info -a /dev/ttyPS1
   udevadm info -a /dev/iio:device0
   udevadm info -a /dev/iio:device1

| 

Deployment notes
================

* Treat Red Pitaya as a general-purpose Linux endpoint in your security architecture.
* Be careful with writable FAT32 partitions: FAT32 lacks Linux ownership and permission attributes.
* Validate startup scripts and service files before enabling auto-start.
* Treat remote deployment channels as privileged paths and secure them accordingly.
* If internet-facing access is required, add your own hardening layer (firewalling, segmentation, authentication controls, and service minimization).

| 

Responsibility for custom changes
=================================

Red Pitaya provides the official OS and software as a development-focused platform and delivers updates for supported releases.

Official repositories:

* :rp-github:`Main ecosystem repository <RedPitaya/tree/master>`
* :rp-github:`FPGA repository <RedPitaya-FPGA/tree/master>`

License and usage terms for source code are defined in the relevant repository files (for example ``LICENSE`` and ``README``) and may differ between components.

If users apply third-party modifications or custom changes (for example custom kernels, altered services, external packages, custom images, or permission model changes), 
they are responsible for validating security, compatibility, and operational behavior of those modifications in their environment.

Red Pitaya cannot guarantee functionality or security properties of third-party or user-modified software stacks.

|

Related sections
================

* :ref:`Network configuration <network>`
* :ref:`Service management <service_management>`
* :ref:`Troubleshooting <sw_troubleshooting>`
