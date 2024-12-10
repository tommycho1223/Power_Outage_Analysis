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

For this analysis, I will utilize the Power Outage dataset obtained from the Purdue University website. The original dataset contains 1,540 rows and 57 columns; however, the top six rows include metadata and lack meaningful content. To prepare the dataset for analysis, I will drop the first five rows and use the remaining rows as column names, as the original dataset does not have valid column headers. The dataset records major power outage data from January 2000 to July 2016.
On the other hand, I will only focus on the columns I need for this analyze, so I will drop the columns I don't use.
| Column | Description |
| **'year'** | The year when the outage event occurred |
| **'month'** | The month when the outage event occurred |
