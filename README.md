# The Impact of Climate Anomalies on Power Outages Across the United States

This is a project for DSC 80 at UCSD

By Tommy Li (wal016@ucsd.edu)

## Introduction

Power outages can really shake things up, affecting daily life, business operations, and essential services. Figuring out what causes these outages, their patterns, and how long they last is crucial. This knowledge helps improve power grid reliability, reduces downtime, and better supports the communities impacted.

In this project, I am diving into a dataset on major power outage events across the United States to uncover valuable insights and predict how long outages might last and what factors influence them.

The central question of this project is:

### **"What factors influence the duration of power outages, and can we predict outage durations based on these factors?"**

This question matters because knowing how long outages will last helps utility companies plan their resources wisely. It also gives communities a chance to prepare and handle disruptions more smoothly.

I have broken the project into several key steps, each focusing on a different part of the dataset to deepen our understanding and sharpen our models:

1. Data Cleaning and Exploratory Data Analysis
2. Assessment of Missingness
3. Hypothesis Testing
4. Framing a Prediction Problem
5. Basline and Final Model
6. Fairness Analysis

For this analysis, I will utilize the Power Outage dataset obtained from the Purdue University website. The original dataset contains 1,540 rows and 57 columns. 

The dataset records major power outage data from January 2000 to July 2016. I will only focus on the columns I need for this analyze, so I will drop the columns I don't use.

Below are the column I keep for analyzing.

| Column | Description |
| ----- | ---------------------------|
| **year** | The year when the outage event occurred |
| **month** | The month when the outage event occurred |
| **us_state** | All the states in the U.S. |
| **climate_region**' | U.S. Climate regions as specified by National Centers for Environmental Information |
| **anomaly_level** | The oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season |
| **popden_urban** | Population density of the urban areas (persons per square mile) |
| **popden_rural** | Population density of the rural areas (persons per square mile) |
| **outage_start** | Power outage start date and time (I combined date and time in this new column from the original columns) |
| **outage_restoration** | Power outage store date and time (I combined date and time in this new column from the original columns) |
| **cause_category** | Categories of all the events causing the major power outages |
| **cause_category_detail** | the event categories causing the major power outages |
| **outage_duration** | Duration of outage events (in minutes) |

## Data Cleaning and Exploratory Data Analysis

- In order to make it easier to understand the dataset and accurately observe and analyze data, I drop the first five rows because they are meaningless for this analysis (the top fifth rows include metadata and lack meaningful content). I take the sixth row in the original dataset as the column names. Also, I drop columns that are not relevant to the topic I am analyzing next, respectively: **OBS**, **POSTAL.CODE**, **NERC.REGION**, **HURRICANE.NAMES**, **DEMAND.LOSS.MW**, **CUSTOMERS.AFFECTED**, **RES.PRICE**, **COM.PRICE**, **IND.PRICE**, **TOTAL.PRICE**, **RES.SALES**, **COM.SALES**, **IND,SALES**, **TOTAL.SALES**, **RES.PERCEN**, **COM.PERCEN**, **IND.PERCEN**, **RES.CUSTOMERS**, **COM.CUSTOMERS**, **IND.CUSTOMERS**, **TOTAL.CUSTOMERS**, **RES.CUST.PCT**, **COM.CUST.PCT**, **IND.CUST.PCT**, **PC.REALGSP.STATE**, **PC.REALGSP.USA**, **PC.REALGSP.REL**, **PC.REALGSP.CHANGE**, **UTIL.REALGSP**, **UTIL.CONTRI**, **PI.UTIL.OFUSA**, **POPULATION**, **POPPCT_URBAN**, **POPPCT_UC**, **POPDEN_UC**, **AREAPCT_URBAN**, **AREAPCT_UC**, **PCT_LAND**, **PCT_WATER_TOT**, **PCT_WATER_INLAND**, **CAUSE.CATEGORY.DETAIL**, **CLIMATE.CATEGORY**, **OUTAGE.START.TIME**, **OUTAGE.START.DATE**, **OUTAGE.RESTORATION.TIME** and **OUTAGE.RESTORATION.DATE**.

- Before I drop **OUTAGE.START.TIME**, **OUTAGE.START.DATE**, **OUTAGE.RESTORATION.TIME** and **OUTAGE.RESTORATION.DATE**, I combine **outage_start_date** and **outage_start_time** into a new outage_start column, and **outage_restoration_date** and **outage_restoration_time** into a new **outage_restoration** column for easier analysis.

- After filtering and cleaning, we reset the DataFrame index to maintain a clean, ascending index. Below are the first few rows of the cleaned dataset.

| year | month | us_state | climate_region | anomaly_level | popden_urban | popden_rural | cause_category | outage_duration | outage_start | outage_restoration |
| ----- | --- | --------- | ------------------ | ---- | ----- | ---- | ------------ | ----- | ----------------------- | ----------------------- |
| 2011 | 7 | Minnesota | East North Central | -0.3 | 2279 | 18.2 | severe weather | 3060 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00 |
| 2014 | 5 | Minnesota | East North Central | -0.1 | 2279 | 18.2 | intentional attack | 1 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00 |
| 2010 | 10 | Minnesota | East North Central | -1.5 | 2279 | 18.2 | severe weather | 3000 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00 |
| 2012 | 6 | Minnesota | East North Central | -0.1 | 2279 | 18.2 | severe weather | 2550 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00 |
| 2015 | 7 | Minnesota | East North Central | 1.2 | 2279 | 18.2 | severe weather | 1740 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00 |

- I notice that **climate_region** has non-trivial missingness, and I will analyze it using the permutation test in **Assessment of Missingness** to see if the missingness depended on **us_state**. For now, I decided not to remove the `NaN` values because I want to investigate the reason for the missing values. However, I will create other datasets without `NaN` values when I perform permutation tests to ensure accuracy in the analysis.

### Univariate Analysis

In my exploratory Data Analysis, I deicded to analyze power outage durations in the U.S. I generate a histogram to show the distribution of power outage durations in minutes.

<iframe
  src="assets/duration_hist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

It seems that the majority of outage durations occur within around `20,000` minutes, with a sharp decline in frequency as the durations increase. There are outliers, such as around `50,000`, `80,000`, and over `100,000` minutes. The data is right-skewed, which means it is rare to have a very long-duration of a power outage.

Next, I perform the other histogram to present the distribution of anomaly levels associated with power outages.

<iframe
  src="assets/anomaly_lvl_distri.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Positive anomaly levels mean warm ocean temperatures, while negative anomaly levels mean cool ocean temperatures. The graph shows the range from approximately `-1.5` to `+2.5`, and negative anomaly levels cause more power outages. It is interesting that it seems around `+2.0` is ideal temperatures to avoid power outages.

### Bivariate Analysis

To gain more insight into power outage occurrences across the United States, I constructed the distribution of power outage durations across different climate regions.

<iframe
  src="assets/duration_by_climate_region.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the boxplot, I can see that different climate regions show varying outage durations. `Northeast`, `South` and `Central` regions appear to have more frequent long-duration outages, while `West North Central` have shorter durations with fewer outliers. On the other hand, significant outliers, which are above `40,000` hours are present across multiple regions such as `East North Central` and `Northeast`, etc. The median outage duration is relatively low across most regions, which means while long outages occur, the majority of outages are shorter in duration.

I create a scatter plot to show the relationship between anomaly level and outage duration.

<iframe
  src="assets/duration_vs_anomaly_level.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

There does not appear to be a strong relationship between anomaly level and outage duration. The outage Duration vs. Anomaly Level graph indicates that a positive anomaly level likely has a lower power outage durations and smaller number of occurrences. A few extreme values (above 40,000 hours) are present at both positive and negative anomaly levels but `-0.4` and `-0.5` have significant outliers. Despite most points clustering around lower outage durations, there are several extreme outliers with very long outage durations.

## Assessment of Missingness

To dig deeper into the missing data in the **climate_region**, I run a permutation test to see if this missingness is linked to the **us_state**. I believe that **climate_region** is **NMAR** (Not Missing at Random).

**Null Hypothesis**: The missingness in **climate_region** is independent of **us_state**, and any observed patterns are due to random chance.

**Alternative Hypothesis**: The missingness in **climate_region** depends on **us_state**.

<iframe
  src="assets/missing_area.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The figure shows that the missing values ​​in **climate_region** belong to *Hawaii* and *Alaska*, which makese sense because *Hawaii* and *Alaska* are not in continental U.S. Therefore, they don't count as either regions. The missing **climate_region** data is likely **NMAR** because it appears to be influenced by the exclusion of Hawaii and Alaska from the typical climate region classification.

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