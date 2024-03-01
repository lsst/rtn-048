.. image:: https://img.shields.io/badge/rtn--048-lsst.io-brightgreen.svg
   :target: https://rtn-048.lsst.io/
.. image:: https://github.com/lsst/rtn-048/workflows/CI/badge.svg
   :target: https://github.com/lsst/rtn-048/actions/

#####################################################################
Requirements for Scheduler and Observing Progress Monitoring Software
#####################################################################

RTN-048
=======

Tools for monitoring and exploring scheduler behavior and survey progress are an important element of Rubin Observatory infrastructure. A variety of users will require these data in the form of tables and visualizations, and require access to them in a variety of contexts and environments. Examples include telescope operators monitoring the scheduler for problems during observing, managers preparing reports for funding agencies, and scientists from science working groups reviewing the effectiveness or survey strategy as implemented. Requirements for tools for creating and providing access to scheduler and survey progress data and visualizations are collected here.

**Links:**

- Publication URL: https://rtn-048.lsst.io/
- Alternative editions: https://rtn-048.lsst.io/v
- GitHub repository: https://github.com/lsst/rtn-048
- Build system: https://github.com/lsst/rtn-048/actions/

Build this technical note
=========================

You can clone this repository and build the technote locally if your system has Python 3.11 or later:

.. code-block:: bash

   git clone https://github.com/lsst/rtn-048
   cd rtn-048
   make init
   make html

Repeat the ``make html`` command to rebuild the technote after making changes.
If you need to delete any intermediate files for a clean build, run ``make clean``.

The built technote is located at ``_build/html/index.html``.

Publishing changes to the web
=============================

This technote is published to https://rtn-048.lsst.io/ whenever you push changes to the ``main`` branch on GitHub.
When you push changes to a another branch, a preview of the technote is published to https://rtn-048.lsst.io/v.

Editing this technical note
===========================

The main content of this technote is in ``index.rst`` (a reStructuredText file).
Metadata and configuration is in the ``technote.toml`` file.
For guidance on creating content and information about specifying metadata and configuration, see the Documenteer documentation: https://documenteer.lsst.io/technotes.
