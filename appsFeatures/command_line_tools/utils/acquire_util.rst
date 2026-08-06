.. _acquire_util:

.. TODO add acquire_p - split trigger mode functionality https://github.com/RedPitaya/RedPitaya/tree/master/Test/acquire_p

Signal acquisition utility
==========================

The Red Pitaya signal can be acquired using the acquire command line utility. It returns raw samples from the ADC buffer to the standard output. Usage instructions:

The acquire utility works in two different modes:

- **Normal mode** - the utility waits for a trigger event and then acquires the data on all channels.
- **Split trigger mode** - the acquisition is split between channels, with each channel having its own trigger. The acquire_p utility is used for this mode.

|

acquire utility usage
----------------------

Standard ``acquire`` utility usage is as follows:

.. tabs::

    .. group-tab:: OS version 3.00

        .. code-block:: console

            redpitaya> acquire
            acquire Version: 3.00-809-bce7a0397

            Usage: acquire [OPTION]... SIZE <DEC>

            --equalization  -e    Use equalization filter in FPGA (default: disabled).
            --shaping       -s    Use shaping filter in FPGA (default: disabled).
            --bypass        -b    Bypass shaping filter in FPGA.
            --gain1=g       -1 g  Use Channel 1 gain setting g [lv, hv] (default: lv).
            --gain2=g       -2 g  Use Channel 2 gain setting g [lv, hv] (default: lv).
            --tr_ch=c       -t c  Enable trigger by channel. Setting c use for channels [1P, 1N, 1A, 2P, 2N, 2A, EP (external channel), EN (external channel)].
                                    P - positive edge, N -negative edge, A - any edge. By default trigger no set
            --tr_level=c    -l c  Set trigger level (default: 0).
            --hex           -x    Print value in hex.
            --volt          -o    Print value in volt.
            --int                 Interrupt-based operation mode.
            --avg                 Outputs the average value for the values in the buffer.
            --calib         -c    Disable calibration parameters
            --16bit               Enables 16Bit mode
            --hk            -k    Reset houskeeping (Reset state for GPIO). Default: disabled
            --axi           -a    Enable AXI interface. Also enable housekeeping reset. Default: disabled
            --debug         -g    Debug registers. Default: disabled
            --offset              Offset relative to the trigger pointer [-16384 .. 16384]
            --version       -v    Print version info.
            --help          -h    Print this message.
                SIZE                Number of samples to acquire [0 - 16384].
                DEC                 Decimation [1,2,4,8,16,...] (default: 1). Valid values are from 1 to 65536

    .. group-tab:: OS version 2.00

        .. code-block:: console

            redpitaya> acquire
            acquire Version: 2.07-651-631657660

            Usage: acquire [OPTION]... SIZE <DEC>

            --equalization  -e    Use equalization filter in FPGA (default: disabled).
            --shaping       -s    Use shaping filter in FPGA (default: disabled).
            --bypass        -b    Bypass shaping filter in FPGA.
            --gain1=g       -1 g  Use Channel 1 gain setting g [lv, hv] (default: lv).
            --gain2=g       -2 g  Use Channel 2 gain setting g [lv, hv] (default: lv).
            --tr_ch=c       -t c  Enable trigger by channel. Setting c use for channels [1P, 1N, 2P, 2N, EP (external channel), EN (external channel)].
                                    P - positive edge, N -negative edge. By default trigger no set
            --tr_level=c    -l c  Set trigger level (default: 0).
            --version       -v    Print version info.
            --help          -h    Print this message.
            --hex           -x    Print value in hex.
            --volt          -o    Print value in volt.
            --avg                 Outputs the average value for the values in the buffer.
            --calib         -c    Disable calibration parameters
            --hk            -k    Reset houskeeping (Reset state for GPIO). Default: disabled
            --axi           -a    Enable AXI interface. Also enable housekeeping reset. Default: disabled
            --debug         -g    Debug registers. Default: disabled
            --offset              Offset relative to the trigger pointer [-16384 .. 16384]
                SIZE                Number of samples to acquire [0 - 16384].
                DEC                 Decimation [1,2,4,8,16,...] (default: 1). Valid values are from 1 to 65536

        Example (acquire 1024 samples with decimation 8, ch1 with at 1:20, results displayed in voltage):

        .. code-block:: console

            redpitaya> acquire 1024 8 -1 lv -o
                -0.175803   0.000977
                0.021975    0.001099
                -0.075693   0.000977
                -0.190453   0.001099
                0.004883    0.001221
                -0.046392   0.001099
                -0.200220   0.000977
                -0.014650   0.001099
                -0.019534   0.001099
                -0.195336   0.000977
                -0.041509   0.001099
                ...
    
    .. group-tab:: OS version 1.00-1.04

        .. code-block:: console

            redpitaya> acquire

            Usage: acquire [OPTION]... SIZE <DEC>

                --equalization  -e    Use equalization filter in FPGA (default: disabled).
                --shaping       -s    Use shaping filter in FPGA (default: disabled).
                --atten1=a      -1 a  Use Channel 1 attenuator setting a [1, 20] (default: 1).
                --atten2=a      -2 a  Use Channel 2 attenuator setting a [1, 20] (default: 1).
                --dc=c          -d c  Enable DC mode. Setting c use for channels [1, 2, B(Both channels)].
                                        By default, AC mode is turned on.
                --tr_ch=c       -t c  Enable trigger by channel. Setting c use for channels [1P, 1N, 2P, 2N, EP (external channel), EN (external channel)].
                                        P - positive edge, N -negative edge. By default trigger no set
                --tr_level=c    -l c  Set trigger level (default: 0).
                --version       -v    Print version info.
                --help          -h    Print this message.
                --hex           -x    Print value in hex.
                --volt          -o    Print value in volt.
                --no_reg        -r    Disable load registers config for DAC and ADC.
                --calib         -c    Disable calibration parameters
                    SIZE                Number of samples to acquire [0 - 16384].
                    DEC                 Decimation [1,2,4,8,16,...] (default: 1). Valid values are from 1 to 65536

        Example (acquire 1024 samples with decimation 8, ch1 with at 1:20, results displayed in voltage):

        .. code-block:: console

            redpitaya> acquire 1024 8 -1 20 -o
                -0.175803   0.000977
                0.021975    0.001099
                -0.075693   0.000977
                -0.190453   0.001099
                0.004883    0.001221
                -0.046392   0.001099
                -0.200220   0.000977
                -0.014650   0.001099
                -0.019534   0.001099
                -0.195336   0.000977
                -0.041509   0.001099

    .. group-tab:: OS version 0.99 or older

        .. code-block:: console

            redpitaya> acquire
            acquire version 0.90-299-1278

            Usage: acquire  size <dec>

                size     Number of samples to acquire [0 - 16384].
                dec      Decimation [1,8,64,1024,8192,65536] (default=1).


        Example (acquire 1024 samples with decimation 8):

        .. code-block:: console

            redpitaya> acquire 1024 8
                -148     -81
                -143     -84
                -139     -88
                -134     -82
                ...

To run the signal acquisition utility, perform the following steps:

#.  Load the FPGA image.

    .. tabs::

        .. group-tab:: OS version 2.00 and higher

            .. code-block:: console

                redpitaya> overlay.sh v0.94

        .. group-tab:: OS version 1.04 or older

            .. code-block:: console

                redpitaya> cat /opt/redpitaya/fpga/fpga_0.94.bit > /dev/xdevcfg


#.  Start the console application.
    
    .. code-block:: console

        redpitaya> acquire 1024 8 -1 20 -o
            -0.175803   0.000977
            0.021975    0.001099
            -0.075693   0.000977
            -0.190453   0.001099
            0.004883    0.001221
            -0.046392   0.001099
            -0.200220   0.000977
            -0.014650   0.001099
            -0.019534   0.001099
            -0.195336   0.000977
            -0.041509   0.001099
            ...

Acquisition performance varies between Red Pitaya models. For more information please refer to the Red Pitaya :ref:`Original Gen board comparison <rp-board-comp-orig_gen>` or :ref:`Gen 2 board comparison <rp-board-comp-gen2>`.

.. note::

    If all inputs are measuring the same signal, but produce different measurements, check the calibration of your Red Pitaya using the :ref:`calib utility <calib_util>` or the :ref:`Calibration application <calibration_app>`.

|

acquire_p utility usage
------------------------

The ``acquire_p`` utility is used for capturing data in split trigger mode. It returns raw samples from the ADC buffer to the standard output. Usage instructions:

.. tabs::

    .. group-tab:: OS version 3.00

        .. code-block:: console

            redpitaya> acquire_p
            Version: 3.00-809-bce7a0397

            Application for capturing data in split trigger mode.
            Usage: acquire_p [OPTION]... SIZE <DEC>
                SIZE                Number of samples to acquire [1 - 16384].
                DEC                 Decimation [1,2,4,8,16,17,18...65536] (default: 1). Valid values are from 1 to 65536

            --att1=a              Use Channel 1 attenuator setting a [1, 20] (default: 1).
            --att2=a              Use Channel 2 attenuator setting a [1, 20] (default: 1).

            --tr_ch1=c      -1 c  Enable trigger for ch 1. Setting c use for channels [N (now), 1P, 1N, 1A, 2P, 2N, 2A, EP (ext channel), EN (ext channel)].
            --tr_ch2=c      -2 c  Enable trigger for ch 2. Setting c use for channels [N (now), 1P, 1N, 1A, 2P, 2N, 2A, EP (ext channel), EN (ext channel)].
            --tr_lev1=c           Set trigger level for ch 1 (default: 0).
            --tr_lev2=c           Set trigger level for ch 2 (default: 0).

            --equalization  -e    Use equalization filter in FPGA (default: disabled).
            --shaping       -s    Use shaping filter in FPGA (default: disabled).
            --bypass        -b    Bypass shaping filter in FPGA.
            --version       -v    Print version info.
            --help          -h    Print this message.
            --hex           -x    Print value in hex.
            --volt          -o    Print value in volt.
            --calib         -c    Disable calibration parameters
            --16bit               Enables 16Bit mode
            --int                 Interrupt-based operation mode.
            --hk            -k    Reset houskeeping (Reset state for GPIO). Default: disabled
            --debug         -g    Debug registers. Default: disabled
            --offset              Offset relative to the trigger pointer [-16384 .. 16384]

|

Source code
-----------

The Red Pitaya GitHub repository contains the:

* :rp-github:`source code for the acquire utility <RedPitaya/tree/master/tools/acquire>`
* :rp-github:`source code for the acquire_p utility <RedPitaya/tree/master/tools/acquire_p>`
