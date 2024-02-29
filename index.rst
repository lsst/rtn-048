#####################################################################
Requirements for Scheduler and Observing Progress Monitoring Software
#####################################################################

.. abstract::

   Tools for monitoring and exploring scheduler behavior and survey progress are an important element of Rubin Observatory infrastructure. A variety of users will require these data in the form of tables and visualizations, and require access to them in a variety of contexts and environments. Examples include telescope operators monitoring the scheduler for problems during observing, managers preparing reports for funding agencies, and scientists from science working groups reviewing the effectiveness or survey strategy as implemented. Requirements for tools for creating and providing access to scheduler and survey progress data and visualizations are collected here.



.. Metadata such as the title, authors, and description are set in metadata.yaml


Abstract
========

Tools for monitoring and exploring scheduler behavior and survey progress are an important element of Rubin Observatory infrastructure.
A variety of users will require these data in the form of tables and visualizations, and require access to them in a variety of contexts and environments.
Examples include telescope operators monitoring the scheduler for problems during observing, managers preparing reports for funding agencies, and scientists from science working groups reviewing the effectiveness or survey strategy as implemented.
Requirements for tools for creating and providing access to scheduler and survey progress data and visualizations are collected here.

Introduction
============

This document describes the design constraints and functional requirenemnts on tools for monitoring Rubin Observatory scheduler performants and its progress in completing LSST. 

Related documents
=================

Requirements listed here provide lower level detail on requirments either explicitly listed in other Rubin Observatory requirements documents, or implied by the operations plan.
Specific reporting or monitoring requirements and relevant sections of the operations plan include:

- `LSE-29 <https://ls.st/lse-29>`_: LSST System Requirements Document
  
  - LSR-REQ-0065: Survey Performance Reviews
  - LSR-REQ-0066: Survey Performance Evaluation
  - LSR-REQ-0070: Science Priorities and Survey Monitoring
  - LSR-REQ-0071: Scientific Oversight During Data Collection
- `LSE-30 <https://ls.st/lse-39>`_: Observing System Specification
  
  - OSS-REQ-0033: Survey Planning and Performance Monitoring
  - OSS-REQ-0131: Nightly Summary Products

- `LSE-369 <https://ls.st/lse-369>`_: LSST Scheduler Requirements

  - SCD-REQ-0016: Survey Monitor
  - SCD-REQ-0059: Survey Progress

- RDO-18: PLAN for the OPERATIONS of the VERA C. RUBIN OBSERVATORY and Execution of its LEGACY SURVEY OF SPACE AND TIME

..
  - OSS-REQ-0406: Subsystem Nightly Reporting
  - OSS-REQ-0378: Advanced Publishing of Scheduler Sequence
  - OSS-REQ-0056: System Monitoring & Diagnostics
  - OSS-REQ-0067: Performance & Trend Analysis Toolkit
  - OSS-REQ-0068: Summit Environment Monitoring
  - OSS-REQ-0072: Weather and Meteorological Monitoring
  - OSS-REQ-0078: Maintenance Reporting
  - OSS-REQ-0079: Maintenance Tracking and Analysis
  - OSS-REQ-0314: Subsystem Performance Reporting

  - Section 5.4.2 ("survey scheduling" subsection)

In addition, `RTN-016 <https://rtn-016.lsst.io>`_: *Background and concepts for monitoring survey progress and scheduler performance* provides a description of the tools required, including use cases, example plots and other visualizations, and a list of tools to be used for different use cases.

..  Finally, RTN-037: *Architecture for Scheduler and Observing Progress Monitoring Software* describes the architecture of the software used to fill the requirements described here.

Design Constraints
==================

* The software shall conform to the Rubin code guidelines where applicable (see https://developer.lsst.io/coding/intro.html).
* Third party software must be compatible with the LSST software license (GPLv3).
* Software shall be runnable in a docker container, deployable according to Rubin guidelines for the Rubin Science Platform.

RPF
---

Monitoring and some reports should be available via dashboards. These dashboards will allow calling a function (written by the RPF team) which will return a visualization, potentially along with some summary text.

Objects which should be displayable should include:

* arbitrary instances of bokeh.models.layouts.LayoutDOM, explicitly including those that require a bokeh server to handle python callbacks (e.g.. bokeh.models.plots.Plot or bokeh.server.server.Server, that may or may not require server callbacks)
* static plots, such as a PNG. This includes MAF plots, which may be pre-generated or possibly created on-the-fly.

Multiple versions of dashboards should be able to be created, but the layout of elements will be predefined (i.e. on-the-fly customization of the dashboard itself is not required). Dashboards should be able to be modified by the Rubin teams.

Interaction between dashboard elements is desirable.

FAFF
----

Functionality required for observatory operations should follow the guidelines set by the "First-Look Analysis and Feedback Functionality Breakout Group."
At the time of writing, `SITCOMTN-025 <https://sitcomtn-025.lsst.io>`_: *First-Look Analysis and Feedback Functionality Breakout Group Report* presents their recommendations.
Noteble guidlines include the use of ``bokeh`` for visualizations and suppport for ``papermill``  executed parameterized notebooks.


Functional Requirements
=======================


Tools for performance evaluation and analysis
---------------------------------------------

Introduction
^^^^^^^^^^^^

A variety of visualizations will be required in different contexts.
The context within which the requirements are most general is that of a python module using within `jupyter` notebook to diagnose a problem, or construct a custom, bespoke report of a specific one-time purpose.
Visualizations in other contexts can all be viewed as specializations of a subset of the vizualizations that may need to be supplied by this module.
Therefore, requirements for different kinds of visualizations are identified here, and referenced in requirements for other contexts.


Table of astronomical events
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of astronomical events on a given date.
Such event should include:

- Sunrise, Sunset, Nautical Twilight, and Astronomical Twilight times (in LST, MJD, UTC, and civil time).
- Moon phase, coordinates, rise, transit, and set times (in LST, MJD, UTC, and civil time).


Sky maps
^^^^^^^^

The ``schedview`` module shall include a function to create a sky map showing the distribution of important quantities over the sky.
This map should include sliders, dropdowns, or text entry boxes that let the reader select the  time, quantity to be mapped, projection, and projection parameters.

Parameters to be mapped can include:

- Sky brightness
- Basis functions of the Feature Based Scheduler at a given time, such as
  
  - Limiting magnitude
  - Moon avoidance
  - Footprint
- Moon location
- Ecliptic
- Galactic plane
- Horizon
- Airmass limit
- Bright stars and planets
- Visits from simulated surveys
- Current MAF metric values (produced by a healpix slicer)
- Simulated final MAF metric values (produced by a healpix slicer)

Survey reward and feasibility plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot showing the rewards and feasibility of surveys included in an instance of a scheduler, over a specified time window. This plot should provide two views, one illustrating maximum reward per survey as a function of time and another illustrating feasibility of each survey as a function of time. This provides insights into how the scheduler is prioritizing different kinds of observations at different times.


Schedule table
^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of scheduled sequences of visits for a given scheduler, time, and database of visits (simulated, actual, or hybrid) or a subset of visits such as DDF sequences.
This table should include columns reporting detailed information on the sequence of visits scheduled for the specified night, such as (for example):
  
- filters,
- start time,
- finish time,
- expected depth,
- airmass
- hour angle,
- angle with moon,

.. Optionally, if provided with a database of visits, the table should include for each DDF:
 - the time of the most recent visit to each field before the specified night.
 - start time of actual visits
 - finish time of actual visits
 - depth
 - maximum airmass
 - the next expected visit date after the specified night.

Further visualization for some subsets of these scheduled visits (such as a "field survey" or "DDF survey") shall be supported by a function within ``schedview`` to show:

- Times of the night over which the survey is feasible.
- Transit, rise, and set (at airmass limits) for each field (may be redundant with feasibility times).
- Sky brightness, limiting magnitude, airmass, and angle with moon distributions for the visits


Visit table
^^^^^^^^^^^

The ``schedview`` module shall include a function to show a table of visits in a given database of visits, in a given time window. This has similar aspects to the schedule table above, however would be more appropriate for representing acquired visits, rather than scheduled visits.

Columns in this table should include information about scheduler state as well as the kinds of information in the schedule table above. This may include:

- scheduler call,
- scheduler name,
- input telemetry to the scheduler

The "scheduler call" identifies which call to the scheduler resulted in the visit. For example, all visits that are part of the same "blob" will have the same scheduler call identifier. What this identifier looks like is TBD; it might be the start time of the first visit scheduled by the call.

Visit property histogram
^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create histograms of visit properties for a given database of visits (simulated, actual, or hybrid), for a given range of times.
Examples of properties that should be plotted include columns from the `opsim` outputs and derived quantities computed by MAF "stackers". Examples include declination, R.A., H.A., LST, airmass, sky brightness, limiting magnitude, and seeing.

Visit property time series plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create plots of visit properties for a given database of visits (simulated, actual, or hybrid) as a function of time.
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_.

Visit property hourglass plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create hourglass plots of visit properties for a given database of visits (simulated, actual, or hybrid). (See RTN-016 figures 4 and 5.)
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_

Summary Statistics and Metrics
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module will be able to calculate and display several statistics on both the currently acquired and future predicted visits. These statistics may be MAF summary metrics or simpler summaries, and may be calculated within a given time window or as a function of time. Comparing values from simulations to actual visits should be supported.
Example summary statistics may include:

- Total elapsed clock time and total open shutter time
- Total t_eff
- Numbers of visits in each filter
- Numbers of visits in each survey


