.. _troubleshooting_advanced:

###################################
Step 7: Advanced troubleshooting
###################################

This is the final section of the troubleshooting guide. If you have completed all the previous sections and the problem persists this section contains some 
niche troubleshooting steps that may help you resolve the issue.

.. contents:: Table of Contents
    :local:
    :depth: 2
    :backlinks: top

|

Prerequisites
===============

Use this section if:

* **The problem persists throughout all previous troubleshooting steps**. If you have checked all the sections above and the problem persists, but you are able to access the web interface and the applications.


Step-by-step advanced troubleshooting
=====================================

The most likely cause is a more specific hardware or software issue. Please check the following in order:

Software and firmware checks
----------------------------

1. **Updating from 1.04 or older OS**. If you have updated from 1.04 (or older) to 2.00 OS or a higher version, check GitHub issues |#250| and |#254|.
#. **Nightly builds**. Check the :ref:`nightly builds changelog <nightly_builds>` for any relevant updates.
#. **Known software issues**. Check the known software issues in the :ref:`software section <known_sw_issues>`.

|

Hardware-specific checks
------------------------

4. **UART TX pin on E2 connector**. For **Original Gen** board models, check if the UART TX pin on the :ref:`E2 <E2_orig_gen>` connector is driven high (3V3) before or during the boot sequence. The board will boot normally, but you will not be able to access the web interface or connect through :ref:`SSH <ssh>`.
#. **Known hardware issues**. Check the known hardware issues for :ref:`Original generation <known_hw_issues_orig_gen>` and :ref:`Gen 2 <known_hw_issues_gen2>` boards.

|

System recovery checks
----------------------

5. **Kernel Panic**. A kernel panic occurs when the operating system detects an unrecoverable error — typically caused by a program corrupting or accessing invalid memory, hardware issues, or conflicts between software components. Please reinstall the :ref:`latest version of the official Red Pitaya OS <prepareSD>` and check if the problem persists. If it does, please :ref:`contact us <report_problem>`.

|

Next steps
=============

We hope that you have found this troubleshooting guide helpful and that you were able to resolve the issue. If the problem persists, please :ref:`contact us <report_problem>` 
with all the relevant information regarding the problem, including the :ref:`Downloaded system report <system_info>` if possible.

|


.. substitutions

.. |#250| replace:: :rp-github:`#250 <RedPitaya/issues/250>`
.. |#254| replace:: :rp-github:`#254 <RedPitaya/issues/254>`
