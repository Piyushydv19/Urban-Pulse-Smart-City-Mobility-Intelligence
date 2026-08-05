# Urban Pulse -- Bengaluru Smart City Mobility Intelligence Platform

## Overview

**Urban Pulse** is a data engineering and business intelligence project
focused on Bengaluru urban mobility. The project integrates multiple
mobility-related datasets, cleans and transforms them, models them using
a Power BI star-schema approach, and prepares the data for interactive
dashboards.

The current implementation uses **Power BI** for ETL, data modelling,
relationships, DAX measures, and visualization. A **MySQL storage layer
is being integrated** so that the cleaned datasets can be stored
centrally and Power BI can consume the database tables instead of
relying only on local files.

> **Important:** MySQL should be described as implemented only after the
> Power BI model has actually been connected to the MySQL database. The
> existing PBIX was originally built from imported datasets using Power
> Query.

------------------------------------------------------------------------

## Project Objectives

-   Integrate heterogeneous Bengaluru mobility datasets.
-   Perform ETL and data quality checks.
-   Standardize schemas and data types.
-   Build fact and dimension tables.
-   Create relationships suitable for Power BI analysis.
-   Create reusable DAX measures.
-   Support traffic, transit, weather, road-safety, demographic, and
    ride-hailing analysis.
-   Build an executive-level Smart City Mobility dashboard.

------------------------------------------------------------------------

## Data Sources / Domains

### 1. BMTC Public Transit (GTFS)

Used for Bengaluru public transport analysis.

Main GTFS entities include:

-   Agency
-   Routes
-   Trips
-   Stops
-   Stop Times
-   Calendar / Service information
-   Shapes

In the Power BI model, `Fact_Trips` acts as the primary transit fact
table, supported by `Fact_StopTimes`.

### 2. Traffic Data

Used for traffic and congestion-related analysis in Bengaluru.

The traffic data supports the **Traffic Dashboard** and can be connected
to the common date dimension when a valid date field is available.

### 3. Weather Data

Weather data was collected using **Open-Meteo**.

It can support analysis of:

-   Temperature
-   Rainfall / precipitation
-   Wind
-   Weather conditions
-   Date-wise weather trends

Weather API integration can also be used later for current/live weather
information.

### 4. Road Safety / Accident Data

Bengaluru Traffic Police station-wise accident data is used for
road-safety analysis.

It supports:

-   Accident totals
-   Fatal/non-fatal analysis
-   Police-station-wise analysis
-   Zone/sub-division analysis
-   Accident hotspot identification

### 5. Demographics

Demographic data is used to understand the population context of
Bengaluru and support demographic dashboard analysis.

Depending on the source granularity, demographic information is modelled
using district/demographic tables.

### 6. Ride Hailing -- Namma Yatri

The Namma Yatri dataset provides **ward-level aggregated ride-hailing
statistics** for Bengaluru.

Available metrics include:

-   Ward
-   Searches
-   Searches which got estimates
-   Searches for quotes
-   Searches which got quotes
-   Bookings
-   Completed trips
-   Search-to-estimate rate
-   Estimate-to-search-for-quotes rate
-   Quote acceptance rate
-   Quote-to-booking rate
-   Cancelled bookings
-   Booking cancellation rate
-   Conversion rate
-   Drivers' earnings
-   Average distance per trip
-   Average fare per trip
-   Distance travelled

### Ride-Hailing Date Limitation

The available Namma Yatri dataset is **aggregated by ward** and does not
contain a Date or Timestamp field.

Therefore:

-   It is **not connected to `Dim_Date`**.
-   A fake date was not created.
-   It is connected through `Dim_Ward`.
-   Time-series ride-hailing analysis cannot be performed with this
    particular source.

This decision preserves data integrity.

### 7. Bike Sharing

A suitable authentic Bengaluru bike-sharing trip dataset with the
required coverage was not available during the current implementation.

Rather than mixing data from another city or fabricating records,
bike-sharing has been left as a possible future extension.

------------------------------------------------------------------------

## ETL Workflow

The datasets were processed using **Power Query in Power BI**.

Typical ETL steps included:

1.  Importing source files.
2.  Inspecting column names and data types.
3.  Removing unnecessary columns where required.
4.  Checking duplicate records.
5.  Handling missing/null values according to analytical importance.
6.  Standardizing column names and formats.
7.  Correcting numeric, date, time, and text data types.
8.  Creating dimension/fact structures.
9.  Loading transformed queries into the Power BI data model.
10. Creating and validating relationships.
11. Creating reusable DAX measures.

### Null Value Handling

Null values are handled according to the role of the column:

-   Key/relationship columns are checked carefully because null keys can
    affect relationships.
-   Missing categorical values may be represented as `Unknown` where
    appropriate.
-   Missing numeric values may be replaced only when a meaningful
    business rule exists.
-   Invalid/unusable rows may be removed where necessary.
-   Values are not blindly replaced if doing so could distort the
    analysis.

------------------------------------------------------------------------

## Power BI Query Mode

The existing Power BI implementation primarily uses **Import Mode**.

The source datasets are processed through **Power Query**, and after
`Close & Apply`, the queries are loaded into the Power BI semantic model
as tables.

Workflow:

`Source Dataset → Power Query → Cleaning/Transformation → Power BI Tables → Relationships → DAX Measures → Dashboards`

Import Mode was selected because most project datasets are static or
periodically refreshed rather than continuously streamed.

------------------------------------------------------------------------

## Data Model

The project uses a star-schema-oriented design with multiple related
analytical domains.

Important tables include:

### Transit

-   `Fact_Trips`
-   `Fact_StopTimes`
-   `Dim_Route`
-   `Dim_Stop`
-   `Dim_Agency`
-   `Dim_Calendar`
-   `Dim_Shapes` / shape-related data where applicable

### Common / Temporal

-   `Dim_Date`

### Traffic & Weather

-   Traffic fact/data table
-   Weather table

### Road Safety

-   Bengaluru Traffic Police station-wise accident table
-   `Dim_Geography`

`Dim_Geography` contains fields such as:

-   Station
-   Sub-division
-   Zone

### Demographics

-   `Fact_Demographics`
-   `Dim_District`

### Ride Hailing

-   `Fact_RideHailing_Ward`
-   `Dim_Ward`

Relationship:

`Dim_Ward[Ward] (1) → (*) Fact_RideHailing_Ward[Ward]`

------------------------------------------------------------------------

## Relationship Design

Relationships are created only where a valid common key exists.

General modelling principles:

-   Dimension tables are normally on the **one** side.
-   Fact tables are normally on the **many** side.
-   Single-direction filtering is preferred where appropriate.
-   Fact-to-fact relationships are avoided.
-   Relationships are not forced between datasets that have incompatible
    granularity.

For example, ride-hailing data uses **Ward**, while the road-safety
geography table uses **Station / Sub-division / Zone**. They are not
directly connected without an authentic mapping between those geographic
levels.

------------------------------------------------------------------------

## Measures

A dedicated `Measure_Table` is used to organize DAX measures.

The Measure Table is intentionally not a transactional data table. It
acts as a central container for measures that calculate values from the
underlying fact tables.

Example ride-hailing measures include:

``` dax
Total Searches =
SUM(Fact_RideHailing_Ward[Searches])
```

``` dax
Total Bookings =
SUM(Fact_RideHailing_Ward[Bookings])
```

``` dax
Completed Trips =
SUM(Fact_RideHailing_Ward[Completed Trips])
```

``` dax
Cancelled Bookings =
SUM(Fact_RideHailing_Ward[Cancelled Bookings])
```

``` dax
Driver Earnings =
SUM(Fact_RideHailing_Ward[Drivers' Earnings])
```

Additional measures are created for the other domains as required by the
dashboards.

------------------------------------------------------------------------

## Planned Dashboard Pages

The final Power BI report is organized into the following dashboard
areas:

### Executive Dashboard

City-level overview and important KPIs from multiple domains.

### Traffic Dashboard

Traffic patterns, congestion, location-based traffic analysis, and
related KPIs.

### BMTC Dashboard

Routes, trips, stops, service information, and public-transit analysis.

### Weather Dashboard

Temperature, precipitation, wind, and weather trends.

### Road Safety Dashboard

Accidents, fatal/non-fatal incidents, station-wise and zone-wise
analysis.

### Demographics Dashboard

Population and available demographic indicators.

### Ride Hailing Dashboard

Namma Yatri searches, bookings, completed trips, cancellations, driver
earnings, average fare, average distance, and ward-wise performance.

------------------------------------------------------------------------

## MySQL Integration

The target architecture includes **MySQL** as a centralized data-storage
layer.

The intended architecture is:

`Raw Datasets / APIs → ETL → MySQL → Power BI → Dashboards`

Once implemented, the MySQL database can contain the cleaned fact and
dimension tables, and Power BI can connect using:

`Home → Get Data → MySQL Database`

For the current project size, **Import Mode** is suitable even when
MySQL is used as the source. DirectQuery can be considered when
near-real-time querying of a frequently changing database is required.

------------------------------------------------------------------------

## Team Workflow

A master PBIX file contains the cleaned datasets, model, relationships,
and measures.

Team members should:

1.  Download the latest master PBIX.
2.  Verify that tables, relationships, and measures are available.
3.  Work only on their assigned dashboard page.
4.  Avoid changing existing Power Query transformations, relationships,
    table names, or measures unless coordinated.
5.  Return their completed PBIX/dashboard page for final integration.

The final dashboard pages are consolidated into one master Power BI
report for presentation.

------------------------------------------------------------------------

## Current Project Status

### Data Engineering

-   Data collection: **Completed for current scope**
-   Data cleaning and transformation: **Completed**
-   Schema standardization: **Completed**
-   Fact/dimension modelling: **Completed**
-   Power BI relationships: **Completed**
-   DAX measure preparation: **Completed / extended as dashboards
    require**
-   Power BI model validation: **Completed**
-   Ride-hailing integration: **Completed**
-   MySQL migration/integration: **In Progress**
-   Bike-sharing integration: **Future scope due to source-data
    limitation**

### Visualization

Dashboard development is the current phase.

------------------------------------------------------------------------

## Technology Stack

-   **Power BI Desktop** -- ETL, modelling, DAX, visualization
-   **Power Query** -- Data cleaning and transformation
-   **DAX** -- Measures and analytical calculations
-   **MySQL** -- Centralized database layer being integrated
-   **MySQL Workbench** -- Database management and SQL validation
-   **GTFS** -- BMTC public-transit data format
-   **Open-Meteo** -- Weather data/API source
-   **GitHub** -- Project collaboration and repository management

------------------------------------------------------------------------

## Future Enhancements

-   Complete MySQL-backed data pipeline.
-   Automate ingestion from APIs.
-   Add scheduled refresh.
-   Add live/current weather API integration.
-   Add a Bengaluru bike-sharing source if reliable public data becomes
    available.
-   Add timestamp/time dimensions when suitable time-granular source
    data is available.
-   Add geographic mapping between wards, traffic zones, and
    administrative boundaries.
-   Deploy the final report through Power BI Service where
    licensing/environment permits.

------------------------------------------------------------------------

## Project Note

The project prioritizes **data integrity over forced integration**.
Tables are related only when valid keys and compatible data granularity
exist. Where a source lacks fields such as date/timestamp, those
relationships are intentionally not fabricated.
