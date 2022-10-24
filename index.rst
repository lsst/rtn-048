:tocdepth: 1

.. sectnum::

.. Metadata such as the title, authors, and description are set in metadata.yaml

.. TODO: Delete the note below before merging new content to the main branch.

.. note::

   **This technote is a work-in-progress.**

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
Specific requirements and sections of the operations plan include:

- LST-29: LSST System Requriments Document
  
  - LSR-REQ-0065: Survey Performance Reviews
  - LSR-REQ-0066: Survey Performance Evaluation
  - LSR-REQ-0070: Science Priorities and Survey Monitoring
  - LSR-REQ-0071: Scientific Oversight During Data Collection
- LST-30: Observing System Specification
  
  - OSS-REQ-0033: Survey Planning and Performance Monitoring
  - OSS-REQ-0131: Nightly Summary Products

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

- LSE-369: LSST Scheduler Requirements

  - SCD-REQ-0016: Survey Monitor
  - SCD-REQ-0059: Survey Progress

- RDO-18: PLAN for the OPERATIONS of the VERA C. RUBIN OBSERVATORY and Execution of its LEGACY SURVEY OF SPACE AND TIME

  - Section 5.4.2 ("survey scheduling" subsection)

In addition, RTN-016: *Background and concepts for monitoring survey progress and scheduler performance* provides a description of the tools required, including use cases, example plots and other visualizations, and a list of tools to be used for different use cases.
Finally, RTN-037: *Architecture for Scheduler and Observing Progress Monitoring Software* describes the architecture of the software used to fill the requiremnts described here.

Design Constraints
==================

FAFF
----

Functionality required for observatory operations should follow the guidlines set by the "First-Look Analysis and Feedback Functionality Breakout Group."
At the time of writing, SITCOMTN-025: *First-Look Analysis and Feedback Functionality Breakout Group Report* presents their recommendations.
Noteble guidlines include the use of ``bokeh`` for visualizations and suppport for ``papermill``  executed parameterized notebooks.

Functional Requirements
=======================

Tools for performance evaluation and analysis
---------------------------------------------

Introduction
^^^^^^^^^^^^

A variety of vizualizations will be required in different contexts.
The context withing which the requirements are most general is that of a python module using within `jupyter` notebook to diagnose a problem, or construct a custum, bespoke report of a specific one-time purpose.
Visualizations in other contexts can all be viewed as specializations of a subset of the vizualizations that may need to be supplied by this module.
Therefore, requirements for different kinds of visualizations are identified here, and referenced in requirements for other contexts.

.. note::
   RTN-016 section 4.5

Table of astronomical events
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of astnonomical events on a given date.
Such event should include:

- Sunrise, Sunset, Nautical Twilight, and Astronomical Twilight times (in LST, MJD, UTC, and civil time).
- Moon phase, coordinates, rise, transit, and set times (in LST, MJD, UTC, and civil time).

Weather table
^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of wheather conditions as a function of time.
Columns should include cloud cover, seeing, temperature, humidity, wind direction, and wind speed.

Possible sources for the weather should include simulated weather from `rubin_sim`, recorded weather data, and forecasts.

Sky map
^^^^^^^

The ``schedview`` module shall include a function to create a sky map showing the distribution of important quantities over the sky.
This map should include sliders, dropdowns, or text entry boxes that let the reader select the  time, quantity to be mapped, projection, and projection parameters.

Parameters to be mapped should include:

- Sky brightness
- Common basis functions
  
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

DDF schedule table
^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of scheduled DDF sequences of visits for a given scheduler, time, and database of visits (simulated, actual, or hybrid).
This table should include columns reporting:

- the time of the most recent visit to each field before the specified night.
- detailed information on any sequences of visits scheduled for the specified night, including
  
  - filters,
  - start time,
  - finish time,
  - expected depth,
  - maximum airmass,
  - maximum hour angle,
  - minimum angle with moon,
- the next expected visit date after the specificed night.

Optionally, if provided with a database of visits, the table should include for each DDF:

 - start time of actual visits
 - finish time of actual visits
 - depth
 - maximum airmass

DDF candence plot
^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot that represents sets of DDF visits as bars for each night.
See RTN-016 figure 6.

Field survey plot
^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of fields in any single field surveys included within a given scheduler, for a specified night.

This data should include:

- Times of the night over which the survey is feasible.
- Transit, rise, and set (at airmass limits) for each field (maybe redundant with feasibility times).
- Sky brightness, limiting magnitude, airmass, and angle with moon for each field (partially redundant with feature value plots).

Visit table
^^^^^^^^^^^

The ``schedview`` module shall include a function to show a table of visits in a given database of visits, in a given time window.
Columns in this table should include the scheduler call, start time, scheduler name, filter, coordinates, reward, hour angle, slew time, limiting magnitude, airmass, sky brightness, angle with moon. The "scheduler call" identifies which call to the scheduler resulted in the visit. For example, all visits that are part of the same "blob" will have the same scheduler call identifier. What this identifier looks like is TBD; it might be the start time of the first visit scheduled by the call.

Visit property histogram
^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create histograms of visit properties for a given database of visits (simulated, actual, or hybrid), for a given range of times.
Examples of properties that should be plotted include columns from the `opsim` outputs and derived quantities computed by MAF "stackers". Examples inclued declination, R.A., H.A., LST, airmass, sky brightness, limiting magnitude, and seeing.

Visit property time series plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create plots of visit properties for a given database of visits (simulated, actual, or hybrid) as a function of time.
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_.

Visit property box plot
^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create box plots of visit properties for a given database of visits (simulated, actual, or hybrid).
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_. Groupings that determine the content of each box shall include divisitions by hour, night, month, or year, and should include other options.

Visit property hourglass plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create hourglass plots of visit properties for a given database of visits (simulated, actual, or hybrid). (See RTN-016 figures 4 and 5.)
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_.

Loaded filter plot
^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot showing the loaded filters as a function of time, given a database of visits (simulated, actual, or hybrid) and a time window.

Pre-scheduled visit table
^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table of pre-scheduled visits in a given scheduler and time range.
Columns should include the start and stop times, filters used, and airmass ranges.

Survey reward and feasibility plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot showing the rewards and fealibility of surveys included in an instance of a scheduler, over a specified time window. The plot should include two panels, one above the other, with time on horizontal axis. In upper panel, one line per survey, lines should show the (maximum) reward as a function of time. In lower panel, horizontal bars (one per survey) should show when each survey is feasible.

Telescope model validation plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot of the actual vs. modeled values for each property modeled by the instrument model in ``rubin_sim``, for all visits in a visit database over a given time window.
Validated values should include slew time, filter change time, shutter time, readout time, total overhead between successive exposures, sky brightness, delivered PSF width (given DIMM seeing), and achieved depth.

Visit summary statistics table
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a table showing basic statistics for visits in a visit database, within a given time window.
Statistics should include:
- Total wall clock time
- Total open shutter time
- Total t_eff
- Total efficiency (total exposure time/total observing time)
- Effective efficiency (total t_eff/total observing time)
- Numbers of visits in each filter
- Numbers of visits in each survey

Achieved summary metric time series plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot showing the values of summary metrics as a function of time.
There should be an option to overplot the values from the simulated baseline.

Predicted summary metric time series plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot showing the predicted final value of summary metrics (selectable by a drop down) as a function of time. That is, the value at time ``t`` should show the value as calculated from a database that includes actual visits before `t`, and simualted visits after `t`.
There should be an optional horizontal line showing the value from a simulated baseline.

Other MAF metric plot
^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create arbitrary MAF metric plots given a database of (achieved, simulated, or hybrid) visits.

Night plan
----------

Introduction
^^^^^^^^^^^^

.. note::
   RTN-016 section 4.2

Execution at Rubin Observatory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Creation of the night plan shall be executable at the observatory site, using site infrastructure.

Automatic creation
^^^^^^^^^^^^^^^^^^

A night plan shall be created before each night of observing.
The plan should be created automatically, either with minimial human interaction (e.g. hitting a button on a web page) or none at all.

Night plan content
^^^^^^^^^^^^^^^^^^

The night plan should include the following:

- `Table of astronomical events`_ for the planned night
- `Sky map`_ for times during the planned night, including simulated visits for the night
- `DDF schedule table`_ for the planned night
- `Field survey plot`_ for the planned night
- `Pre-scheduled visit table`_ for the planned night
- `Survey reward and feasibility plot`_ for the planned night.

The night plan should alse include the following plots for the results of one or `opsim` simulations of the night:

- `Visit property histogram`_ for each simulation of the planned night
- `Loaded filter plot`_ for each simulation of the planned night
- `Visit table`_ for the simulation of the planned night

Night report
------------ 

Introduction
^^^^^^^^^^^^

RTN-016 section 4.4 gives an overview of the contents of scheduler and progress related elements in the night report.
Higher level requirments related to the night report include SE-30/OSS-
REQ-0131, LSE-30/OSS-REQ-0406, LSE-61/DMS-REQ-0096, and LSE-61/DMS-REQ-0097.

Automatic creation
^^^^^^^^^^^^^^^^^^

A night report shall be created after each night of observing.
The report should be created automatically, either with minimial human interaction (e.g. hitting a button on a web page) or none at all.

.. note::
  A separate piece of infrastructure to create the scheduler related elements of the night report may not be what we want to do.
  A better approach may be to put this outside the scope of scheduler work, and just have ``schedview`` supply the tools to create the elements of the report, and have them called by another system.

Night report content
^^^^^^^^^^^^^^^^^^^^

The night report should include:

- `Visit summary statistics table`_
- `Table of astronomical events`_
- `Sky map`_ including visits made in the night
- `Telescope model validation plot`_
- `DDF schedule table`_
- `Visit property histogram`_
- `Visit property time series plot`_
- `Visit table`_

Progress and survey performance reports
---------------------------------------

Introduction
^^^^^^^^^^^^

RTN-016 section 4.6

Automatic creation
^^^^^^^^^^^^^^^^^^

A progress and survey performance report shall be created automatically after each night of observing.

Static figures
^^^^^^^^^^^^^^

The progress and survey performance report generation shall create static files for each figure, suitable for inclusion in printed documents.
This requirement *does not* preclude generation of interactive and dynamic plots in addition to the static forms.

Progress and survey performance report content
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- `Visit summary statistics table`_
- `Sky map`_ including options to show current and simulated final healpix metrics
- `Achieved summary metric time series plot`_
- `Predicted summary metric time series plot`_
- `Visit property hourglass plot`_
- `Other MAF metric plot`_ for achieved and extrapolated databases of visits, for the most important set of metrics (e.g. the depth-area plot).

