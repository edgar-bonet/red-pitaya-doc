.. _FPGA_install_vivado_2025_1:

###############################################
Installation of Vitis 2025.1 and Vivado 2025.1
###############################################

This installation tutorial will guide you through the installation of Vitis 2025.1 and Vivado 2025.1 on your computer or virtual machine. The tutorial is intended for anyone
who wants to use the FPGA of the Red Pitaya board for Red Pitaya OS 3.00 or higher.

With older versions of the Red Pitaya OS, it was necessary to install Vivado 2020.1 and Xilinx SDK 2019.1 separately. Vivado was used for building the FPGA bitstreams, 
while Xilinx SDK was used for building the ARM software. With Red Pitaya OS 3.00 or higher, Vitis 2025.1 is used for both tasks.

Installing Vitis will also install the corresponding Vivado version, so we will have access to both tools at the same time.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Software compatibility
=======================

Vivado 2025.1 is necessary to build the FPGA bitstreams for the following Red Pitaya OS versions:

* Red Pitaya OS 3.00 or higher

|


Creating an AMD account
============================

Regardless of the operating system we use, we will need to create a free **AMD account**.

    .. figure:: ../img/Vivado-install/Licence-AMD-sign-in.png
        :width: 400

Previous versions of Vivado required a Vivado WebPACK license, however, Vivado 2025.1 does not require a license for Zynq 7000 series.

|

Download Vitis 2025.1
=======================

.. note::

    During the creation of this tutorial, the latest version of Vitis was 2026.1. It is expected that the AMD Vitis download page will be updated, so the positioning of the download links may change.
    However, the general pattern of the download and installation process should remain the same.

1.  Head to :ref:`AMD Vitis download <vitis_downloads>` and click on the **Download Vitis** button.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_download_1.png
        :width: 1000
        :align: center

#.  Select **Vitis 2025.1** from the dropdown menu on the right side of the page. This will redirect you to the download page for Vitis 2025.1.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_download_2.png
        :width: 1000
        :align: center

#.  Scroll down slightly until you see the **Unified installer for FPGA & Adaptive SoC Tools - 2025.1 - Jun 4, 2025**. If the dropdown menu is not already 
    expanded click on the down arrow to expand it.

    .. note::

        Vitis 2025.1 has an Update 1 version dated 17 Sep 2025, which supports some extra devices, but is unnecessary for Zynq 7000 series and will only 
        take up extra space on your computer. Therefore, we will be using the original version of Vitis 2025.1.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_download_3.png
        :width: 1000
        :align: center

#.  Depending on the computer OS you are using, select the appropriate **AMD Unified installer for FPGA & Adaptive SoC Tools - 2025.1** for your 
    operating system from the three available options:

    * Use **Windows Self Extracting Web Installer** for Windows.
    * Use **Linux Self Extracting Web Installer** for Linux.

    .. note::

        In case the Self Extracting Web Installers do not work for any reason, please use the **Unified Installer**.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_download_4.png
        :width: 1000
        :align: center

#.  After clicking on the link, you will be redirected to AMD sign in page. Log in using your AMD username and password. If you don't have an AMD account, 
    you will have to create one now (it's free).

    .. figure:: ../img/Vivado-install/Licence-AMD-sign-in.png
        :width: 500
        :align: center

#.  The sign in page will redirect you to the **Name and Address verification** page of the Downloads center. Fill in the required information and click 
    on the **Download** button at the bottom of the page.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_5.png
        :width: 1000
        :align: center

#.  The download will start automatically. Since the file is only about 200 MB, it should not take too long to download.

    .. note::

        In case of **Unified Installer**, the file will take significantly longer to download. Ensure the internet connection is stable and the download 
        is not interrupted. If the download is interrupted, you will have to start the download process from the beginning.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_6.png
        :width: 400
        :align: center

#.  In case a **Unified Installer** was downloaded, extract the .tar.gz file using your preferred method.

At this point you should have an extracted Vitis 2025.1 installer. Now we will focus on the installation process for each operating system.

|

Installing Vitis 2025.1
=========================

The installation process is exactly the same for Windows and Linux, so we will cover both operating systems in the same section.

.. note::

    Since Vitis 2025.1 likely has a fixed number of supported Linux Ubuntu versions and does not account for future releases, it may be necessary to "fake" 
    the Ubuntu version during the installation process in the future. Understandably, this is not ideal, but it is a common practice for software that 
    has not been updated to support newer operating systems. If you encounter any issues during the installation process, please refer to the official 
    AMD Vitis documentation or seek assistance from the Red Pitaya community.

The installation process is quite straightforward. You just have to run the installer and follow the instructions.

#.  Double-click the downloaded **FPGAs_AdaptiveSoCs_Unified_SDI_2025.1_0530_0145_Win64.exe** or **FPGAs_AdaptiveSoCs_Unified_SDI_2025.1_0530_0145_Lin64.bin**
    installer. For unified installers, head to the extracted folder and run the **xsetup.exe** or **xsetup.bin** file to start the installation process.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_1.png
        :width: 300

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_2.png
        :width: 500

#.  Once the installer starts, you will see a pop-up window informing you that a new version of the installer is available. Click **Continue** to proceed 
    with the installation.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_3.png
        :width: 600

#.  The first screen shows the installation requirements. Including the supported operating systems. Click **Next** to continue. Since Unified installers 
    do not use internet connection, it is possible that a warning will pop up informing you that the download servers are not accessible. Close the warning 
    and continue with the installation.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_4.png
        :width: 1000
        :align: center

#.  On **Web installers**, you will be asked to sign in to your AMD account. On older Vivado versions this was a major breaking point, since the installer 
    could not connect to the AMD servers to authenticate the user. If this does not work, please use the **Unified installer**, which skips the sign in 
    process.

    Select **Download and Install Now** and click **Next**.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_5.png
        :width: 1000
        :align: center

#.  Select **Vitis** as we will install the full environment, which also enables software development for the ARM processor. If you only plan to use 
    Vivado for FPGA development, you can select **Vivado** only. Click **Next** to continue.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_6.png
        :width: 1000
        :align: center

#.  Now we will be presented with the installation options. Make sure to select the **Zynq-7000 All Programmable SoC** option located under **SoCs**. 
    This is the only option we need for Red Pitaya boards. Leave the other ticks as they are. If you need additional options for other development, s
    elect them as needed. Keep in mind that selecting unnecessary options will increase the installation time and disk space usage. 
    
    Click **Next** to continue.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_7.png
        :width: 1000
        :align: center


#.  Check all the license agreement boxes on the loooooong list of the license agreements and click **Next**.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_8.png
        :width: 1000
        :align: center

#.  Now we have to select the installation directory. The default installation directory is **C:/Xilinx**, but we can change it to a different one if necessary. 
    Here we are using the **C:/Programs/Xilinx** directory, but feel free to change it. 
    
    Make sure to select **All users** option under the **Apply shortcut & file associations**, otherwise the shortcuts will only be available for the 
    admin user account.
    
    Click **Next**.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_9.png
        :width: 1000
        :align: center

#.  Check the installation summary and click **Install** to start the installation process.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_10.png
        :width: 1000
        :align: center

#.  Wait for the installation to finish. This may take a while, depending on the selected options, speed of your computer, and internet connection. The 
    installation process will download the necessary files from the AMD servers, so a stable internet connection is required.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_11.png
        :width: 1000
        :align: center

#.  Once the installation is complete, you will see the following screen. Click **OK** to finish the installation process.

    .. figure:: ../img/Vivado-install/2025.1/Vitis_2025_install_12.png
        :width: 1000
        :align: center
    
#.  Afterwards, a License Manager window will pop up. Since Zynq 7000 series does not require a license, we will just close the Window. If you are using 
    a different FPGA, please follow the instructions in the License Manager to obtain a license.

#.  **Windows** - Add the Vivado and Vitis ``bin`` folders to the ``PATH`` environment variable so the ``vivado`` and ``vitis`` commands are available in any 
    new shell you open.

    If you installed Vivado in the default location, add this folder:

    .. code-block:: text

        C:\Xilinx\2025.1\Vivado\bin
        C:\Xilinx\2025.1\Vitis\bin

    If you used a custom installation directory such as ``C:\Programs\Xilinx``, add this folder instead:

    .. code-block:: text

        C:\Programs\Xilinx\2025.1\Vivado\bin
        C:\Programs\Xilinx\2025.1\Vitis\bin

    Add the folder to **User variables > Path** to update the current user only, or to **System variables > Path** to make it available for all users.
    Updating the system PATH requires administrator privileges. After changing the PATH, close and reopen any shells so they pick up the new setting.

#.  **Linux** - As a Linux user, we recommend adding the **.settings64-Vivado.sh** and **.settings64-Vitis.sh** scripts to your shell configuration file 
    (e.g., **.bashrc** or **.zshrc**), so you can start Vitis and Vivado from the terminal by typing **vitis** or **vivado**.

    .. code-block:: shell

        echo 'source /opt/Programs/Xilinx/2025.1/Vivado/.settings64-Vivado.sh' >> ~/.bashrc
        echo 'source /opt/Programs/Xilinx/2025.1/Vitis/.settings64-Vitis.sh' >> ~/.bashrc
        source ~/.bashrc

    .. note::

        The paths in the commands above may vary depending on the installation directory you selected during the installation process. Adjust the 
        paths accordingly.

#.  As the final step, we should check our *Language and Region settings* on our Ubuntu/Linux computer and make sure we have a **Format** that uses 
    **a dot (“.”) as a decimal separator** (the United Kingdom or the United States will work). **Vivado demands the use of a dot as the decimal separator**, 
    which can lead to problems with Bitstream generation as Vivado will not recognize certain parts of the model.

#.  We are now ready to use Vivado 2025.1 on both Windows and Linux operating systems.

|
