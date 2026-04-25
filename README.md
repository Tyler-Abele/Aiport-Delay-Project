# DS 4320 Project 2: Airport Delay Analysis

This repository contains a fully estbalished secondary dataset built using implicit schema to predict the likelihood of airport delays across the United States. to make th edata more consistent and modeling-friendly, the project standardizes multiple sources with different time granularities into a common airport-month analytical unit. A noSQL approach has been taken with regards to data creation and storage, with all data being accesable on MongoDB Atlas. A machine learning pipeline loads the data into MongoDB and then pulls from it to create a modeling dataset. The goal is the support interpretable delay forecasting for policymakers, airline employees, and airport planners, in order to improve air travel in the United States.
 -- refinement needed once problem starts to get developed more

 ## Author
 - Name: Tyler Abele
 - NetID: xxe9ff

[![DOI](https://zenodo.org/badge/1215430778.svg)](https://doi.org/10.5281/zenodo.19666622)

## Quick Links:
| **Resource** | **Link** |
| --- | --- |
| Press Release | [link](./press-release.md) |
| Data (OneDrive) | [link](https://myuva-my.sharepoint.com/:f:/g/personal/xxe9ff_virginia_edu/IgCLnlIOBJo2QZeVYyAgAtxkAeT9kr4PN24246pOsqPX1qM?e=iuqgva)|
| Data (repo) | [link](./data/) |
| Pipeline Notebook | [link](./pipeline/master_pipeline.ipynb)|
| Pipeline Markdown | [link](./pipeline/master_pipeline.md)|
| Liscense | [link](./LICENSE)|

## Problem Definition

### General Problem
Predicting airport delays

### Specific Problem
Predicting whether a domestic U.S. commercial flight will be delayed by more than 15 minutes based on historical on-timeperformance data, weather conditions, airline, route, time of day, and day of week.

### Motivation
Flight delays cost the U.S. economy over $30 billion annually and affect millions of passengers each year. For travelers, a delayed flight can mean missed connections, lost business meetings, and ruined vacations. For airlines, delays cascade through the network and compound into massive operational costs. Despite this, most passengers have no way to assess the likelihood of a delay before booking. A reliable delay prediction model would empower travelers to make smarter booking decisions and help airlines proactively manage operations.

### Rational for Refinement
The general problem of "predicting outcomes" is too broad to be actionable, it
could mean anything from sports scores to election results. I narrowed to flight delays because the domain has abundant, well-structured public data from the Bureau of Transportation Statistics (BTS), a clear binary outcome (delayed vs. on-time using the FAA's standard 15-minute threshold), and features that map naturally to the document model, each flight is a self-contained document with nested airline, airport, schedule, and weather information. The problem is also well-suited for the classification techniques covered in DS 3021/4021, making the ML component straightforward to implement.

### Press Release
**Headline** New Model Predicts Flight Delays With [X]% Accuracy — Helping Travelers
Avoid the Gate Wait
-- Update accruacy with final performance

see the full press release: [press-release](./press-release.md)

## Domain Exposition

### Terminology
| **term:** | **definition**|
| -------- | --------- |
|BTS| Bureau of Transportation Statistics: the federal agency within the U.S. Department of Transportation that collects and publishes data on airline on-time performance, cancellations, and delays |
|FAA Federal Aviation Administration| the U.S. government agency responsible for regulating civil aviation, air traffic control, and safety standards|
|On-Time| A flight that arrives within 14 minutes of its scheduled arrival time, per the DOT's official definition|
|Delayed| A flight that arrives 15 or more minutes after its scheduled arrival time, the standard threshold used by the DOT for reporting purposes|
|Carrier Delay| A delay caused by circumstances within the airline's control, such as maintenance problems, crew scheduling, baggage loading, or fueling issues|
|Weather Delay| A delay caused by significant meteorological conditions such as thunderstorms, snow, fog, or wind that prevent safe flight operations|
|NAS Delay| National Airspace System delay — caused by air traffic control limitations, airport operations, heavy traffic volume, or airspace restrictions|
|Late Aircraft Delay| A delay caused by the aircraft arriving late from a previous flight, creating a cascading or ripple effect through the schedule|
|Security Delay| A delay caused by security-related events such as terminal evacuations, re-screening of passengers, or equipment failures at checkpoints|
|OTP| On-Time Performance — the percentage of an airline's flights that arrive on time over a given period, used as a key reliability metric|
|Hub| A major airport where an airline concentrates its operations and routes connecting passengers through that central point|
|Load Factor| The percentage of available seats on a flight that are actually occupied by passengers, a measure of how full the plane is|
|Taxi Time| The time an aircraft spends on the ground moving between the gate and the runway, split into taxi-out (departure) and taxi-in (arrival)|
|Block Time| The total time from gate departure to gate arrival, including taxi time, flight time, and any ground holds|
|OOOI (Out-Off-On-In) |the four timestamps that define a flight's block time: Out (gate departure), Off (wheels up), On (wheels down), In (gate arrival)|
|Codeshare| A commercial arrangement where two or more airlines sell seats on the same flight under their own flight numbers|
|IATA Code| A two-letter airline designator or three-letter airport code assigned by the International Air Transport Association, used globally for identification|
|METAR (Meteorological Aerodrome Report) | a standardized weather observation format used at airports worldwide, reporting conditions like visibility, wind, temperature, and cloud cover|
|Slot Control| A system used at congested airports where airlines must obtain permission to operate takeoffs and landings during specific time windows|
|Ground Stop| An FAA traffic management initiative that halts all departures to a specific airport, typically due to weather, volume, or equipment outages at the destination|
|Ground Delay Program| An FAA traffic management program that assigns specific departure times to flights heading to a congested airport, spreading out arrivals to reduce holding patterns|
|Binary Classification| A machine learning task where the model predicts one of two outcomes, in this case delayed (15+ minutes) or on-time|
|Feature Engineering| The process of creating new input variables from raw data to improve model performance, such as computing historical delay rates for a specific route or time of day|

### Domain Overview
The domain of this project is air transportation operations and delay forecasting within the broader field of transportation analytics. This field sits at the intersection of operations research, logistics, and data science, and is concerned with understanding the patterns, causes, and cascading effects of disruptions in the national airspace system. In practice, this involves the systematic collection and analysis of flight-level performance data, which flights departed on time, which were delayed, by how much, and why, alongside contextual factors like weather conditions, airport congestion, and airline scheduling practices to identify patterns that can inform both predictive models and operational decisions. The insights generated from this kind of analysis are used by airlines for schedule planning and crew management, by airports for capacity and resource allocation, by the FAA for traffic flow management, and increasingly by passengers and booking platforms to make more informed travel decisions.

### Background Readings

All readings are kept in this onedrive folder: [readings](https://myuva-my.sharepoint.com/:f:/g/personal/xxe9ff_virginia_edu/IgDiF7pND8vqR5clS9k3_wkUARD6hb1RwIu3KQcrKjtF6kk)

|**title** | **brief summary** | **link** |
| ------  | ------- | ----- |
|Technical Directive No39 On-Time 2025| official documentation describing all fields in the Reporting Carrier On-Time Performance dataset, including delay cause breakdowns and measurement definitions, very technical read (literally federal documentation)| [link](https://myuva-my.sharepoint.com/:b:/r/personal/xxe9ff_virginia_edu/Documents/Final%20Project/readings/Technical%20Directive%20No%20%2039%20On-Time%202025.pdf?csf=1&web=1&e=bUa2Sl) |
| FAA NextGen Weather Integration Overview| explains how the FAA incorporates weather data into traffic flow management and how weather drives the majority of NAS delays| [link](https://myuva-my.sharepoint.com/:u:/g/personal/xxe9ff_virginia_edu/IQAsmqTdcjJXR4iNCBijbTNZAeogN8HAdbPpLAms9vnRMlE?e=tUbmHN) |
| Machine Learning Approaches to Flight Delay Prediction | surveys different ML techniques applied to flight delay prediction including random forests, gradient boosting, and neural networks, comparing accuracy across feature sets| [link](https://myuva-my.sharepoint.com/:u:/g/personal/xxe9ff_virginia_edu/IQAOnq50ubJVQY5L2j4odeu-AebBtxrEX1wUREPZvF_rgxk?e=CNiWBa) |
| Economic Impact of Flight Delays | quantifies the annual cost of flight delays to airlines, passengers, and the broader economy at over $30 billion | [link](https://myuva-my.sharepoint.com/:u:/r/personal/xxe9ff_virginia_edu/Documents/Final%20Project/readings/Aviation%20economics%20and%20policy%20projects.url?csf=1&web=1&e=re84oc) |
| Air Travel Consumer Report | industry group summary of delay causes, seasonal trends, and airline-level performance comparisons | [link](https://myuva-my.sharepoint.com/:u:/g/personal/xxe9ff_virginia_edu/IQAdW1V1ek-8Sqqw8tuyzZneATa3qOf8BFdESC2OnTvGAr0?e=eqJGk2) |

## Data Creation

### Data Creation Process
The primary dataset for this project comes from the Bureau of Transportation
Statistics (BTS) Reporting Carrier On-Time Performance database, which is
publicly available at transtats.bts.gov. The BTS collects this data directly
from U.S. certified air carriers that are required by federal law to report
monthly on-time performance metrics. I downloaded three months of data
(January, February, and March 2025) as individual CSV files, each containing
roughly 500,000 flight records with fields covering flight dates, carrier
information, origin and destination airports, scheduled and actual times,
delay durations broken down by cause, and cancellation/diversion indicators.

Because each monthly CSV contains approximately 500K rows and the MongoDB
Atlas free tier has a 512 MB storage limit, I randomly sampled 50,000 flights
from the combined pool of all three months (~1.6 million total rows). This
random sampling ensures the dataset maintains a representative spread across
all dates, airlines, airports, and times of day rather than being biased
toward any particular slice. The sampled rows were then transformed from flat
CSV format into nested JSON documents using a Python script with pymongo
and loaded into a MongoDB Atlas collection called "flights" within the
"flight_delays" database.

### Code Table

| File | Description | Link |
| ---- | ----------- | ---- |
| data_loading.ipynb | Loaded the data from the csv files into MongoDB Cluster 0 | [Link](https://github.com/Tyler-Abele/Aiport-Delay-Project/blob/main/loading/data_loading.ipynb) |

### Rationale
The most significant decision is defining "delayed" as an arrival delay of
15 or more minutes. This is the FAA/DOT standard threshold, making it
defensible and widely understood, but it imposes a binary cutoff on a
continuous variable. A flight arriving 14 minutes late is classified as
on-time while one at 15 minutes is delayed. We chose arrival delay over
departure delay because arrival is what ultimately affects the passenger.
The sampling size of 50,000 was chosen to balance storage constraints on
the Atlas free tier against having enough data for meaningful analysis
and model training. The nested document structure groups related fields
(carrier info, origin, destination, schedule, actual times, delays) into
sub-documents, which mirrors how FHIR bundles group related healthcare
data and is a natural fit for the document model. This nesting makes
queries more intuitive but means some aggregation queries require dot
notation to reach nested fields.

### Bias Identification
The BTS dataset only includes carriers that report to the DOT, which covers
all major domestic airlines but may underrepresent very small regional
operators. Additionally, the delay cause fields (CarrierDelay, WeatherDelay,
NASDelay, SecurityDelay, LateAircraftDelay) are self-reported by airlines,
creating an incentive to attribute delays to weather or NAS issues (outside
their control) rather than carrier problems (within their control). The
random sampling process itself could also introduce minor bias if certain
flight types are disproportionately represented in certain months, though
with 50K samples from 1.6M rows this effect is minimal. Finally, the data
only covers three months (Jan-Mar 2025), which is winter-heavy and may
overrepresent weather-related delays compared to a full-year dataset.

### Bias Mitigation
The seasonal bias from using only winter months can be acknowledged as a
limitation and addressed by noting that model performance may differ for
summer months when delay patterns shift from weather (snow, ice) to
convective storms. The self-reporting bias on delay causes is harder to
fix directly, but we can cross-reference with external weather data to
validate whether reported weather delays align with actual adverse conditions.
For the model itself, we use stratified evaluation to check performance
across different carriers and airports, ensuring the model does not
systematically favor high-volume carriers. The random sampling approach
already mitigates temporal clustering bias by ensuring uniform coverage
across all dates.

## Metadata

### Implicit Schema Guidelines

Each document in the flights collection follows this structure. Not all
fields are guaranteed to be present in every document. For example,
delay cause fields (carrier_delay, weather_delay, etc.) are only populated
when a flight is actually delayed, and cancellation_code only appears for
cancelled flights. This is an intentional use of implicit schema: optional
fields appear only when relevant, keeping on-time flight documents leaner.

```json
{
  "flight_date": {
    "date": "2/1/2025 12:00:00 AM",
    "weekday": "6"
  },
  "carrier": {
    "code": "AA",
    "airline_id": 19805,
    "unique_carrier": "AA"
  },
  "flight_number": 1234,
  "origin": {
    "airport_id": 12478,
    "city": "New York, NY",
    "state": "NY"
  },
  "destination": {
    "airport_id": 12892,
    "city_market_id": 32575
  },
  "schedule": {
    "crs_dep_time": "0830",
    "crs_arr_time": "1045",
    "crs_elapsed_time": 195.0
  },
  "actual": {
    "dep_time": "0847",
    "arr_time": "1102",
    "actual_elapsed_time": 195.0,
    "air_time": 178.0,
    "taxi_out": 12.0,
    "taxi_in": 5.0,
    "wheels_off": "0859",
    "wheels_on": "1057"
  },
  "delay": {
    "dep_delay": 17.0,
    "dep_delay_new": 17.0,
    "dep_del15": 1.0,
    "arr_delay": 17.0,
    "arr_delay_new": 17.0,
    "is_delayed": true,
    "carrier_delay": 17.0,
    "weather_delay": 0.0,
    "nas_delay": 0.0,
    "security_delay": 0.0,
    "late_aircraft_delay": 0.0
  },
  "cancelled": false,
  "cancellation_code": null,
  "diverted": false,
  "distance_group": 10
}
```
### Data Summary

| Metric | Value |
|--------|-------|
| Database | flight_delays |
| Collection | flights |
| Total documents | 50,000 |
| Source months | January, February, March 2025 |
| Sampled from | 1.645 million total rows |
| Unique carriers | 14 |
| Unique origin airports | 329  |
| Delayed flights (15+ min) | 13289  |
| Cancelled flights |  174 |

### Data Dictionary

| Field | Type | Description | Example |
|-------|------|------------|---------|
| flight_date.date | string | Date of the flight | "2/1/2025 12:00:00 AM" |
| flight_date.weekday | string | Day of week (1=Mon, 7=Sun) | "6" |
| carrier.code | string | IATA airline code | "AA" |
| carrier.airline_id | int | DOT unique airline identifier | 19805 |
| carrier.unique_carrier | string | Unique carrier code for analysis | "AA" |
| flight_number | int | Flight number | 1234 |
| origin.airport_id | int | DOT origin airport identifier | 12478 |
| origin.city | string | Origin city and state name | "New York, NY" |
| origin.state | string | Origin state abbreviation | "NY" |
| destination.airport_id | int | DOT destination airport identifier | 12892 |
| destination.city_market_id | int | Destination city market identifier | 32575 |
| schedule.crs_dep_time | string | Scheduled departure time (HHMM) | "0830" |
| schedule.crs_arr_time | string | Scheduled arrival time (HHMM) | "1045" |
| schedule.crs_elapsed_time | float | Scheduled flight duration in minutes | 195.0 |
| actual.dep_time | string | Actual departure time (HHMM) | "0847" |
| actual.arr_time | string | Actual arrival time (HHMM) | "1102" |
| actual.actual_elapsed_time | float | Actual flight duration in minutes | 195.0 |
| actual.air_time | float | Time in the air in minutes | 178.0 |
| actual.taxi_out | float | Taxi time from gate to runway in minutes | 12.0 |
| actual.taxi_in | float | Taxi time from runway to gate in minutes | 5.0 |
| actual.wheels_off | string | Time wheels left the ground (HHMM) | "0859" |
| actual.wheels_on | string | Time wheels touched down (HHMM) | "1057" |
| delay.dep_delay | float | Departure delay in minutes (negative means early) | 17.0 |
| delay.dep_delay_new | float | Departure delay, 0 floor (no negatives) | 17.0 |
| delay.dep_del15 | float | Binary: 1 if departure delayed 15+ min | 1.0 |
| delay.arr_delay | float | Arrival delay in minutes (negative means early) | 17.0 |
| delay.arr_delay_new | float | Arrival delay, 0 floor (no negatives) | 17.0 |
| delay.is_delayed | bool | Whether arrival delay >= 15 minutes | true |
| delay.carrier_delay | float/null | Minutes of delay caused by the carrier | 17.0 |
| delay.weather_delay | float/null | Minutes of delay caused by weather | 0.0 |
| delay.nas_delay | float/null | Minutes of delay caused by NAS | 0.0 |
| delay.security_delay | float/null | Minutes of delay caused by security | 0.0 |
| delay.late_aircraft_delay | float/null | Minutes of delay from late arriving aircraft | 0.0 |
| cancelled | bool | Whether the flight was cancelled | false |
| cancellation_code | string/null | Reason for cancellation (A=carrier, B=weather, C=NAS, D=security) | null |
| diverted | bool | Whether the flight was diverted | false |
| distance_group | int | Distance group (1=under 250mi, each +250mi) | 10 |

### Quantifying Uncertainty

| Feature | Description | Uncertainty Notes |
|---------|------------|-------------------|
| delay.arr_delay | Arrival delay in minutes | Heavily right-skewed distribution with long tail. Negative values (early arrivals) are common. Outliers above 300 min exist from severe disruptions |
| delay.dep_delay | Departure delay in minutes | Correlates with arr_delay but not perfectly since flights can make up time in air. Same skew pattern |
| actual.air_time | Minutes in the air | Varies by route distance and wind. Headwinds vs tailwinds introduce ~5-15 min variance on long routes |
| actual.taxi_out | Minutes from gate to runway | Highly variable at congested hubs (JFK, ORD, ATL can exceed 40 min). More consistent at smaller airports |
| actual.taxi_in | Minutes from runway to gate | Generally lower variance than taxi_out. Gate availability introduces occasional spikes |
| schedule.crs_elapsed_time | Scheduled duration | Deterministic (set by airline). No measurement uncertainty, but airlines pad schedules to improve OTP stats |
| delay.carrier_delay | Carrier-attributed delay | Self-reported by airlines. Potential systematic underreporting due to incentive to blame weather/NAS instead |
| delay.weather_delay | Weather-attributed delay | Self-reported. May be overreported relative to actual weather conditions at the time |

