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

	YEAR	MONTH	U.S._STATE	CLIMATE.REGION	...	OUTAGE.RESTORATION.TIME	CAUSE.CATEGORY	CAUSE.CATEGORY.DETAIL	OUTAGE.DURATION
0	2011	7	Minnesota	East North Central	...	20:00:00	severe weather	NaN	3060
1	2014	5	Minnesota	East North Central	...	18:39:00	intentional attack	vandalism	1
2	2010	10	Minnesota	East North Central	...	22:00:00	severe weather	heavy wind	3000
3	2012	6	Minnesota	East North Central	...	23:00:00	severe weather	thunderstorm	2550
4	2015	7	Minnesota	East North Central	...	07:00:00	severe weather	NaN	1740

- 

### Univariate Analysis

<iframe
  src="assets/duration_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It seems that most power outages occur within around `20,000` minutes. Also, there are outliers, such as around `50,000`, `80,000`, and over `100,000` minutes.

<iframe
  src="assets/anomaly_lvl_distri.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Positive anomaly levels mean warm ocean temperatures, while negative anomaly levels mean cool ocean temperatures. The graph shows that negative anomaly levels cause more power outages and range from approximately `-1.5` to `+2.5`. It is interesting that it seems around `+1.5` to `+2.0` are ideal temperatures to avoid power outages.

### Bivariate Analysis

<iframe
  src="assets/duration_by_climate_region.html.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the first graph, we can see that different climate regions show varying outage durations. `Northeast` and `Central` regions appear to have more frequent long-duration outages, while `Hawaii` and `West North Central` have shorter durations with fewer outliers. On the other hand, significant outliers, which are above `40,000` hours are present across multiple regions such as `East North Central` and `Northeast`, etc.

<iframe
  src="assets/duration_vs_anomaly_level.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The outage Duration vs. Anomaly Level graph indicates that a positive anomaly level likely has a lower power outage durations and smaller number of occurrences. A few extreme values (above 40,000 hours) are present at both positive and negative anomaly levels but `-0.4` and `-0.5` have significant outliers.

## Assessment of Missingness


## Hypothesis Testing

**Null Hypothesis**: The frequency of power outages **does not** vary significantly between different climate regions.

**Alternative Hypothesis**: The frequency of power outages **varies significantly** between different climate regions.

If the p-value is **equal or lower** than `0.05`, we reject the null hypothesis, and the mean outage durations vary significantly between different climate regions.

If the p-value is **greater** than `0.05`, we fail to reject the null hypothesis, which means there is no significant evidence that mean outage durations vary between climate regions.

<iframe
  src="assets/mean_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The p-value is greater than `0.05`, so we fail to reject the null hypothesis and there is no significant evidence that the mean outage durations vary significantly between different climate regions. This suggests that any variation in mean outage durations across climate regions could be due to **random chance** rather than a systematic difference.

## Framing a Prediction Problem

My model will predict **the duration of a power outage** based on various factors available in the dataset.

The target variable is `outage_duration`, which indicates the duration of power outage in minutes.

The features are:

`climate_region`: The climate region where the outage occurred. Outages in different regions may have different durations due to local infrastructure and weather conditions.

`cause_category`: The cause of the outage (e.g., equipment failure, severe weather, etc.). Different causes (e.g., severe weather vs. equipment failure) likely result in different outage durations.

`month`: The month the outage occurred.

`anomaly_level`: The El Niño/La Niña index (oceanic anomaly level). The El Niño/La Niña index may influence the severity of weather-related outages.

`outage_start`: The start date and time of the outage.


## Basline Model

I'm going to use two of the features I mentioned in Step 5: Framing a Prediction Problem for the baseline model.`climate_region` and `anmonaly_level`.

The tagrget variable is `outage_duration`.


## Final Model


## Fairness Analysis

<iframe
  src="assets/fairness_model.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>