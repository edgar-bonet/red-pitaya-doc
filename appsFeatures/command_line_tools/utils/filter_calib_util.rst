.. _filter_calib_util:

Filter calibration utility
===========================

Red Pitaya frequency equalisation filter calibration can be accessed and configured using the command line utility. Filter equalisation is used to compensate for 
the frequency response of the Red Pitaya analog front-end.

Usage instructions:

.. tabs::

    .. group-tab:: OS version 3.00 and higher

        .. code-block:: console

            root@rp-f0b1cb:~# filter_calib
            Version: 3.00-809-bce7a0397

            filter_calib -a | -e | -h [-i KK_VALUE] [-g GAIN] [-w]
            --auto                 -a      Automatic filter calibration using internal generator.
            --auto_ext             -e      Automatic filter calibration using external generator (PWM signal of 1kHz 1.8 Vpp).
            --initK=X              -i X    Sets the value for the KK parameter. The default value is 0xdFFFFF.
            --gain=X               -g X    Use gain setting X [LV, HV] (default: LV).
            --write                -w      Write new parameters to eeprom.
            --help                 -h      Print this message.

For more information on calibration and the math behind it, please refer to the :ref:`Frequency calibration <frequency_calibration>` section.

|

Source code
------------

The Red Pitaya GitHub repository contains the :rp-github:`source code for the filter calibration utility <RedPitaya/tree/master/tools/filter_calib>`.
