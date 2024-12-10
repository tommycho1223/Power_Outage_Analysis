# The Impact of Climate Anomalies on Power Outages Across the United States

This is a project for DSC 80 at UCSD

By Tommy Li (wal016@ucsd.edu)

## Introduction

Power outages can really shake things up, affecting daily life, business operations, and essential services. Figuring out what causes these outages, their patterns, and how long they last is crucial. This knowledge helps improve power grid reliability, reduces downtime, and better supports the communities impacted.

In this project, I am diving into power outage data to uncover valuable insights and predict how long outages might last and what factors influence them.

I have broken the project into several key steps, each focusing on a different part of the dataset to deepen our understanding and sharpen our models:

1. Data Cleaning and Exploratory Data Analysis
2. Assessment of Missingness
3. Hypothesis Testing
4. Framing a Prediction Problem
5. Basline and Final Model
6. Fairness Analysis

For this analysis, I will utilize the Power Outage dataset obtained from the Purdue University website. The original dataset contains 1,540 rows and 57 columns. 

The dataset records major power outage data from January 2000 to July 2016. I will only focus on the columns I need for this analyze, so I will drop the columns I don't use.

## Data Cleaning and Exploratory Data Analysis

- In order to make it easier to understand the dataset and accurately observe and analyze data, I drop the first five rows because they are meaningless for this analysis (the top fifth rows include metadata and lack meaningful content). I take the sixth row in the original dataset as the column names. Also, I drop columns that are not relevant to the topic I am analyzing next, respectively: **OBS**, **POSTAL.CODE**, **NERC.REGION**, **HURRICANE.NAMES**, **DEMAND.LOSS.MW**, **CUSTOMERS.AFFECTED**, **RES.PRICE**, **COM.PRICE**, **IND.PRICE**, **TOTAL.PRICE**, **RES.SALES**, **COM.SALES**, **IND,SALES**, **TOTAL.SALES**, **RES.PERCEN**, **COM.PERCEN**, **IND.PERCEN**, **RES.CUSTOMERS**, **COM.CUSTOMERS**, **IND.CUSTOMERS**, **TOTAL.CUSTOMERS**, **RES.CUST.PCT**, **COM.CUST.PCT**, **IND.CUST.PCT**, **PC.REALGSP.STATE**, **PC.REALGSP.USA**, **PC.REALGSP.REL**, **PC.REALGSP.CHANGE**, **UTIL.REALGSP**, **UTIL.CONTRI**, **PI.UTIL.OFUSA**, **POPULATION**, **POPPCT_URBAN**, **POPPCT_UC**, **POPDEN_UC**, **AREAPCT_URBAN**, **AREAPCT_UC**, **PCT_LAND**, **PCT_WATER_TOT**, **PCT_WATER_INLAND**, **OUTAGE.START.TIME**, **OUTAGE.START.DATE**, **OUTAGE.RESTORATION.TIME** and **OUTAGE.RESTORATION.DATE**.

- Below are the columns I keep for the analysis.

| Column | Description |
| ----- | ---------------------------|
| **year** | The year when the outage event occurred |
| **month** | The month when the outage event occurred |
| **us_state** | All the states in the U.S. |
| **climate_region**' | U.S. Climate regions as specified by National Centers for Environmental Information |
| **anomaly_level** | The oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season |
| **climate_category** | The climate episodes corresponding to the years |
| **popden_urban** | Population density of the urban areas (persons per square mile) |
| **popden_rural** | Population density of the rural areas (persons per square mile) |
| **outage_start** | Power outage start date and time (I combined date and time in this new column from the original columns) |
| **outage_restoration** | Power outage store date and time (I combined date and time in this new column from the original columns) |
| **cause_category** | Categories of all the events causing the major power outages |
| **cause_category_detail** | the event categories causing the major power outages |
| **outage_duration** | Duration of outage events (in minutes) |

- Before I drop **OUTAGE.START.TIME**, **OUTAGE.START.DATE**, **OUTAGE.RESTORATION.TIME** and **OUTAGE.RESTORATION.DATE**, I combine them as **outage_start** and **outage_restoration** in order to make the data clearer.

- 

### Univariate Analysis

<iframe
  src="assets/file-name.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis


## Assessment of Missingness


## Hypothesis Testing


## Framing a Prediction Problem


## Basline Model


## Final Model


## Fairness Analysis