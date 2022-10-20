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

Basic astronomical events
^^^^^^^^^^^^^^^^^^^^^^^^^

The night plan shall include a table of astronimical events that will occurr during the night.
These events should include:

- Sunrise, Sunset, Nautical Twilight, and Astronomical Twilight times (in LST, MJD, UTC, and civil time).
- Moon phase, coordinates, rise, transit, and set times (in LST, MJD, UTC, and civil time).

Sky map
^^^^^^^

The night plan shall include a sky map showing the distribution of important quantities over the sky, over the course of the night.
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
  
DDF table
^^^^^^^^^

The night plan shall include information on the DDF fields.
The is data should include:

 - the time of the most recent visit to each field,
 - detailed information on any sequences of visits scheduled for the current night, including filters and start and finish times, and
 - the next expected visit date;

Field survey plots
^^^^^^^^^^^^^^^^^^

The night plan should include data on any single field surveys included in the current scheduler (including DDFs).
This data should include:

- Times of the night over which the survey is feasible.
- Transit, rise, and set (at airmass limits) for each field (maybe redundant with feasibility times).
- Sky brightness, limiting magnitude, airmass, and angle with moon for each field (partially redundant with feature value plots).

Table of pre-scheduled visits
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If there are pre-scheduled visits scheduled for the night, they should be listed in a table including the start and stop times, filters used, and airmass ranges.

Simulated survey plots
^^^^^^^^^^^^^^^^^^^^^^

The night plan shall include a series of visualizations summarizing the results of one or more simulations of the night's observing.
The night plan may include summaries of multiple simulations, covering different weather conditions, for example in good seeing, poor seeing, and forecast wind & seeing. 
These plots should include:

- Distributions (histogram with dropdown of what to plot)

 - airmass,
 - hour angles,
 - slew angles,
 - angles with moon.
  
- A reward and feasibility plot (two panels, one above the other, time on horizontal axis). In upper panel, one line per survey, showing (maximum) reward as a function of time. In lower panel, horizontal bars (one per survey) showing when each survey is feasible.
- A plot showing loaded filter as a function of time (a bar plot with 6 horizontal bars, and time along the horizontal axis)
- A table of visits, with scheduler call, start time, scheduler name, filter, coordinates, reward, hour angle, slew time, limiting magnitude, airmass, sky brightness, angle with moon. The "scheduler call" identifies which call to the scheduler resulted in the visit. For example, all visits that are part of the same "blob" will have the same scheduler call identifier. What this identifier looks like is TBD; it might be the start time of the first visit scheduled by the call.

Weather forecast
^^^^^^^^^^^^^^^^

The night plan should include a weather forecast with clouds, wind, humidity, and seeing.


Night report
------------ 

Introduction
^^^^^^^^^^^^

RTN-016 section 4.4 gives an overview of the contents of scheduler and progress related elements in the night report.
Higher level requirments related to the night report include SE-30/OSS-
REQ-0131, LSE-30/OSS-REQ-0406, LSE-61/DMS-REQ-0096, and LSE-61/DMS-REQ-0097.

Tools for performance evaluation and analysis
---------------------------------------------

Introduction
^^^^^^^^^^^^

.. note::
   RTN-016 section 4.5

Progress and survey performance reports
---------------------------------------

Introduction
^^^^^^^^^^^^

RTN-016 section 4.6
