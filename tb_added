.. Weather table
   ^^^^^^^^^^^^^

..    The ``schedview`` module shall include a function to create a table of weather conditions as a function of time.
     Columns should include cloud cover, seeing, temperature, humidity, wind direction, and wind speed.
     Possible sources for the weather should include simulated weather from `rubin_sim`, recorded weather data, and forecasts. In operations the actual weather conditions will be recorded in the EFD.
    (note: this data is presented real-time at the telescope via LOVE. What do we actually need for later analysis?)



Visit property box plot
^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create box plots of visit properties for a given database of visits (simulated, actual, or hybrid).
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_. Groupings that determine the content of each box shall include divisitions by hour, night, month, or year, and should include other options.
(moved to tbd because we don't have this yet and boxplot may not be the way we want to implement).


Visit property hourglass plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create hourglass plots of visit properties for a given database of visits (simulated, actual, or hybrid). (See RTN-016 figures 4 and 5.)
Properties whose distribution the boxes represent should be the same ones available in `Visit property histogram`_

Telescope model validation plot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``schedview`` module shall include a function to create a plot of the actual vs. modeled values for each property modeled by the instrument model in ``rubin_sim``, for all visits in a visit database over a given time window.
Validated values should include slew time, filter change time, shutter time, readout time, total overhead between successive exposures, sky brightness, delivered PSF width (given DIMM seeing), and achieved depth.
(we need to consider how/what we're going for with this information as well as what we can actually calculate. how much is ROO responsibility?)


(these below are moved to TBD because I am wondering if they are basically -- "call and show MAF metric results, even if single value -- I don't want to be maintaining a calculation of these in two different ways in different places and we have this in MAF already - if it turns out there are significant advantages to reimplementing, fine. Also I generalized in the main file)
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
(moved to TBD because I think we may already have this in 'sky map'?)


-----------


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



----------

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