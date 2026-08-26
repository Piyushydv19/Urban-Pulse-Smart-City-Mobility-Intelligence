# Urban Pulse -- Bengaluru Smart City Mobility Intelligence Platform

## Overview

**Urban Pulse** is a Power BI-based Smart City Mobility Intelligence Platform focused on understanding Bengaluru's urban mobility through data-driven analysis and interactive dashboards.

The project brings together multiple mobility-related datasets, transforms and prepares them using **Power Query**, builds a structured Power BI data model, creates analytical measures using **DAX**, and presents meaningful insights through interactive dashboards.

The platform provides a unified view of Bengaluru's mobility ecosystem, helping analyze traffic, public transport, reliability, ride-hailing, and geospatial mobility patterns.

---

## Project Objectives

- Analyze Bengaluru's urban mobility using real-world datasets.
- Clean and transform data using Power Query.
- Build a structured and scalable Power BI data model.
- Create relationships between relevant datasets.
- Develop reusable DAX measures and KPIs.
- Identify mobility patterns and performance trends.
- Provide interactive and easy-to-understand dashboards.
- Support data-driven understanding of Bengaluru's transportation system.

---

## Data Domains

### 1. BMTC Public Transit

The project uses Bengaluru Metropolitan Transport Corporation (BMTC) public-transit data for analyzing routes, trips, stops, service information, and transit operations.

The transit data includes information related to:

- Routes
- Trips
- Stops
- Stop Times
- Agencies
- Service calendars
- Shapes

This data supports public-transport and reliability analysis.

---

### 2. Traffic

Traffic data is used to understand traffic conditions and mobility patterns across Bengaluru.

The Traffic Dashboard provides insights into:

- Traffic patterns
- Congestion
- Location-based traffic analysis
- Traffic-related KPIs
- Mobility trends

The dashboard helps provide a clear overview of traffic conditions across different areas.

---

### 3. Road Safety

Bengaluru Traffic Police accident data is used for road-safety analysis.

The analysis includes:

- Accident totals
- Fatal and non-fatal incidents
- Police-station-wise analysis
- Zone-wise analysis
- Sub-division analysis
- Accident hotspot identification

Geographic information is used to understand how road-safety patterns vary across Bengaluru.

---

### 4. Demographics

Demographic data is used to provide population and geographic context for mobility analysis.

This helps understand mobility patterns alongside demographic characteristics of different areas.

---

### 5. Ride Hailing -- Namma Yatri

The Namma Yatri dataset provides ward-level ride-hailing information for Bengaluru.

The analysis includes:

- Searches
- Estimates
- Quotes
- Bookings
- Completed trips
- Cancelled bookings
- Conversion rates
- Booking cancellation rates
- Driver earnings
- Average fare
- Average distance
- Distance travelled
- Ward-wise performance

The Ride Hailing Dashboard provides an overview of user demand, booking activity, trip completion, driver earnings, and ward-level mobility performance.

---

## ETL and Data Transformation

The project uses **Power Query in Power BI** for data preparation and transformation.

The major steps include:

1. Importing source datasets.
2. Inspecting and understanding the data.
3. Cleaning unnecessary or inconsistent data.
4. Handling missing values.
5. Removing duplicate records where required.
6. Standardizing column names.
7. Correcting data types.
8. Transforming and preparing datasets for analysis.
9. Creating fact and dimension structures.
10. Loading the transformed data into the Power BI model.

This workflow ensures that the datasets are organized and ready for reliable analysis and visualization.

---

## Power BI Data Model

The project follows a structured, star-schema-oriented approach for organizing the data.

### Transit Tables

- `Fact_Trips`
- `Fact_StopTimes`
- `Dim_Route`
- `Dim_Stop`
- `Dim_Agency`
- `Dim_Calendar`
- `Dim_Shapes`

### Common Tables

- `Dim_Date`

### Road Safety

- Accident data table
- `Dim_Geography`

The geography information includes:

- Station
- Sub-division
- Zone

### Demographics

- `Fact_Demographics`
- `Dim_District`

### Ride Hailing

- `Fact_RideHailing_Ward`
- `Dim_Ward`

The model uses appropriate relationships between dimensions and analytical fact tables to support interactive filtering and dashboard analysis.

---

## DAX Measures

A dedicated measure table is used to organize analytical calculations.

Example measures include:

```DAX
Total Searches =
SUM(Fact_RideHailing_Ward[Searches])

Total Bookings =
SUM(Fact_RideHailing_Ward[Bookings])

Additional DAX measures and KPIs are used across the different dashboard areas to support analytical insights.

Dashboards
Traffic Dashboard

The Traffic Dashboard provides an overview of Bengaluru's traffic conditions and patterns.

It focuses on:

Traffic analysis
Congestion
Location-based insights
Traffic KPIs
Mobility patterns
Reliability Dashboard

The Reliability Dashboard focuses on public-transport service performance and reliability.

It provides insights into transit operations using route, trip, stop, and timing-related information.

The dashboard helps understand how reliably public transportation services operate across the network.

Mobility Dashboard

The Mobility Dashboard provides a broader view of Bengaluru's transportation ecosystem.

It brings together important mobility indicators and helps understand overall urban transportation patterns.

Ride Hailing Dashboard

The Ride Hailing Dashboard analyzes Namma Yatri activity across Bengaluru.

It includes:

Searches
Bookings
Completed trips
Cancellations
Conversion rates
Driver earnings
Average fare
Average distance
Ward-wise performance

This dashboard provides a clear view of ride-hailing demand and performance across different wards.

Geospatial Dashboard

The Geospatial Dashboard focuses on location-based analysis of Bengaluru's mobility data.

It uses geographic information and map-based visualizations to identify spatial patterns, hotspots, and differences between areas.

This helps transform mobility data into location-based insights that are easier to understand and analyze.

Power BI Workflow

The overall project workflow is:

Source Datasets → Power Query → Data Cleaning & Transformation → Power BI Data Model → Relationships → DAX Measures → Interactive Dashboards

Power BI Import Mode is used to provide an efficient environment for working with the project's datasets and dashboards.

Key Features
Interactive Power BI dashboards
Power Query-based ETL
Structured data modelling
Star-schema-oriented design
DAX-based KPIs and measures
Traffic analysis
Public-transport analysis
Reliability analysis
Ride-hailing analysis
Geospatial analysis
Road-safety analysis
Demographic context
Ward-level mobility analysis
Interactive filtering and visualization
Technology Stack
Power BI Desktop -- Data modelling, dashboards, visualization and analytics
Power Query -- Data cleaning, transformation and ETL
DAX -- Measures, KPIs and analytical calculations
GTFS -- Public-transit data
Namma Yatri -- Ride-hailing data
Open-Meteo -- Weather data where applicable
GitHub -- Project repository and collaboration
Team Workflow

The project is maintained through a master Power BI file containing the datasets, data model, relationships, measures, and dashboards.

The team workflow includes:

Maintaining the latest master PBIX file.
Working on assigned dashboard areas.
Maintaining consistent data modelling and Power Query transformations.
Developing and validating DAX measures.
Integrating completed dashboard pages into the master report.
Preparing the final Power BI report for presentation.
Current Project Status
Data Engineering
Data collection: Completed
Data cleaning and transformation: Completed
Schema standardization: Completed
Data modelling: Completed
Power BI relationships: Completed
DAX measures: Completed
Power BI model validation: Completed
Ride-hailing integration: Completed
Visualization
Traffic Dashboard: Developed
Reliability Dashboard: Developed
Mobility Dashboard: Developed
Ride Hailing Dashboard: Developed
Geospatial Dashboard: Developed

The project is focused on refining the dashboards, improving visual presentation, and preparing the final report for demonstration.

Future Enhancements
Improve dashboard interactivity and user experience.
Add additional mobility KPIs and analytical measures.
Enhance geospatial visualizations.
Expand mobility analysis with additional datasets.
Add automated data refresh where applicable.
Continue improving Power BI Service deployment and sharing.
Enhance the platform with additional Smart City mobility insights.
Project Highlights

Urban Pulse demonstrates how multiple urban mobility datasets can be transformed into a structured business intelligence solution using Power BI.

The project combines data engineering, Power Query transformation, data modelling, DAX analytics, and interactive visualization to create a Smart City mobility platform for Bengaluru.

The dashboards provide a comprehensive view of traffic, public transport reliability, overall mobility, ride-hailing activity, road safety, and geospatial mobility patterns, turning raw mobility data into meaningful and actionable insights.
