---
title: 'Bellabeat wellness company case study'
date: 2024-04-15T08:59:50-06:00
draft: false
author: Christian Montenegro
tags:
  - Python
  - Data cleaning
  - Data transformation
  - Data analysis
  - Data visualization
  - Data storytelling
image: 'smart-wearable-device.jpg'
description: A project showcasing my skills on different stages of the data analytics life cycle.
---

## Summary

### Goal

The objective of this project is to use the data analytics life cycle to solve the business task of CBellabeat, a woman wellness company. I use the Fitbit fitness tracker data to support my case. My insights will help to design a marketing campaign to unlock new grow opportunities for the company.


### Process

I begin the data analytic process by obtaining the questions that will drive my analysis. Once I have identified these questions, I retrieve the necessary data from Divvy and merge them into a single feather-formatted file. Before proceeding with the analysis, I verify the data integrity and clean data where necessary. On the analysis phase, I perform data transformation to extract meaningful insights. Once I have identified the key findings, I share my conclusions. Finally, I present my recommendations to drive decision-making.



### Highlights

This project is carried out using Python, using data-science related libraries given its capabilities to handle medium to large sized datasets.

Fitbit users tend to support a sedentary lifestyle consistently across different periods. Other health related measures support the previous result. This suggest a preliminary trend that could poten
 lead to long-term negative effects on their health and overall well-being.

[Click here for the code of this project](https://www.kaggle.com/code/christianmontenegro/bellabeat-case-study)

## Introduction

The current and future market of activity tracking, consumer health informatics, and personally generated health data is poised for significant growth and innovation. As technology continues to advance, we can expect to see more sophisticated wearable devices and mobile applications that not only track a wide array of health metrics but also provide actionable insights and personalized health recommendations. The integration of artificial intelligence and machine learning will enable these tools to offer more accurate and predictive analytics, potentially improving individual health outcomes and prompting timely interventions [1-2]. This situation creates a good opportunity to explore and step into this sector. An excellent business profile is Bellabeat.

## Background

Urška Sršen and Sando Mur founded Bellabeat, a high-tech company that manufactures health-focused smart products. Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits. Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.

## Business task

The company's marketing strategy has invested in traditional advertising media, such as radio, out-of-home billboards, print, and television, but focuses on digital marketing extensively. Bellabeat invests year-round in Google Search, maintaining active Facebook and Instagram pages, and consistently engages consumers on Twitter. Additionally, Bellabeat runs video ads on Youtube and display ads on the Google Display Network to support campaigns around key marketing dates.

Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company.

## Scenario

This capstone project involves a junior data analyst working for Bellabeat in the marketing analyst team. The solution will utilize the six stages proposed in the Google Data Analytics Professional Certificate: Ask, Prepare, Process, Analyze, Share, and Act. These stages define how data is generated, collected, processed, used, and analyzed to achieve business goals. Furthermore, the project will apply these stages to the specific context of Bellabeat's marketing analyst team. The results of this project will aid stakeholders in making informed decisions.
```python
## Load required libraries
# Library for the Python programming language, adding support for large,
# multi-dimensional arrays and matrices, along with a large collection
# of high-level mathematical functions to operate on these arrays
import numpy as np

# Library for data manipulation and analysis.
import pandas as pd

# A comprehensive library for creating static, animated,
# and interactive visualizations in Python
import matplotlib.pyplot as plt

# Data visualization library based on matplotlib. It provides a high-level
# interface for drawing attractive and informative statistical graphics.
import seaborn as sns

# Library that provides a portable way of using operating system
# dependent functionality
import os

# Library that provides a Python API for the Arrow C++ library
# that contains a set of technologies that enable big data systems
# to store, process and move asdata fast.
import pyarrow.feather as feather
```
## Ask

Sršen asks you to analyze smart device usage data in order to gain insight into how consumers use non-Bellabeat smart devices. She then wants you to select one Bellabeat product to apply these insights to in your presentation. These questions will guide your analysis:

What are some trends in smart device usage?
How could these trends apply to Bellabeat customers?
How could these trends help influence Bellabeat marketing strategy?

## Prepare

In this phase, I gather the data, describe it, ensure it has the correct format, credibility, and understand its limitations. I use the Fitbit Fitness Tracker data recommended for this case study. The data was in csv format, which has reading and writing performance limitations. With that in mind, I adopted the feather format. There are some problems with the credibility, so the insights obtained from this data will not be of a completely prescriptive nature.

### Describe data

Sršen encourages you to use public data that explores smart device users’ daily habits. She points you to a specific data set, Fitbit Fitness Tracker Data [3]. This Kaggle data set contains personal fitness tracker from thirty fitbit users n=33
. Thirty three eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits. There are 18 files (csv format) describing the information before mentioned at diferent granularity levels, collected from 03-12-2016 to 05-12-2016 (two months of observations).

### Move and format data

The feather file storage format, has become a popular choice for data storage due to its demonstrated benefits of speed and file size compared to the csv format as discussed in [4-8]. Finally, we save them as a feather file in a separate directory (`cs2-Bellabit-preparation-step`), implemented in the ``convert_csv_to_feather` function.
```python
def convert_csv_to_feather(input_dir, output_dir):
   # Ensure output directory exists
   os.makedirs(output_dir, exist_ok=True)

   # Iterate over all files in input directory
   for filename in os.listdir(input_dir):
       # Check if file represent data on a daily basis
       if filename.endswith(".csv"):
           # Construct full file paths
           input_file = os.path.join(input_dir, filename)
           output_file = os.path.join(output_dir, os.path.splitext(os.path.basename(filename))[0] + '.ftr')

           # Read CSV file into DataFrame
           df = pd.read_csv(input_file)

           # Write DataFrame to Feather file
           feather.write_feather(df, output_file)
   print("All the files located at", input_dir, "were succesfully convert to feather format" )
convert_csv_to_feather(input_dir = "/kaggle/input/fitbit/Fitabase Data 4.12.16-5.12.16/",
    output_dir = "/kaggle/working/cs2-Bellabit-preparation-step")
```
### Check data credibility

The credibility and integrity of our data can be determined using the ROCCC system proposed in the certification:

The data is not reliable because the sample size is insufficient for a quantitative study like this. So, it's important to consider the findings of similar studies, to make an informed estimate of the population proportion and variability.

The data is not original, because it was collected by a third-party provider (Amazon Mechanical Turk).
The data is comprehensive due to its parameters at diferent levels of granularity, matching those from Bellabit's products.

The data is not current, because the data was collected almost eight years ago.

The data is not cited because the third-party provider not cited the original source.

### Understand data limitations

The data was collected six years ago (outdated) with data collected for over two months and the sample size is insufficient (n=33), leading to an irrelevant and unreliable source to know the true population of users habits wearing smart devices. So this case study will be helpful as a quantitative exploratory study or analysis to provide preliminary trends in smart device usage habits.

## Process

To make sure the data we're working with is accurate and reliable, I used certain tools and techniques to process it. First, I looked at each piece of data closely to understand its limitations and challenges. Then, I applied processes to clean the data and remove any errors or inconsistencies. By doing this, we can trust that our results are based on accurate information.

Since the Fitbit Fitness tracker data has observations for two months, we will continue with Python. Python owns data-manipulation libraries that could help me in this phase and provides a good performance with the amount of data available.

### Explore data

To understand the structure of the data, we can load the feather files that contains observations from two months. By examining this data, we can gain insights into the usage patterns of the data collected by Fitbit and make informed decisions about its operation and improvement.

```python
def load_dataframe(filename):
    """Load a DataFrame from a feather file."""
    return pd.read_feather(filename)

# Define file paths
filepaths = {
    "DAILY": ["dailyActivity", "dailyCalories", "dailyIntensities", "dailySteps", "sleepDay", "weightLogInfo"],
    "HOURLY": ["hourlyIntensities", "hourlySteps", "hourlyCalories"],
    "MINUTE": ["minuteIntensitiesNarrow", "minuteStepsNarrow", "minuteMETsNarrow", "minuteCaloriesNarrow", "minuteSleep",
               "minuteIntensitiesWide", "minuteStepsWide", "minuteCaloriesWide"],
    "SECONDS": ["heartrate_seconds"]
}

# Initialize dictionaries to store DataFrames
dataframes = {}

# Loop through each granularity level and load corresponding DataFrames
for granularity, filenames in filepaths.items():
    for filename in filenames:
        full_filename = f"/kaggle/working/cs2-Bellabit-preparation-step/{filename}_merged.ftr"
        df = load_dataframe(full_filename)
        dataframes[f"{filename}"] = df
# Print summary information of data
for name, df in dataframes.items():
    print("--------------------------\n")
    pd.options.display.max_columns = df.shape[1]
    print(name)
    print("SUMMARY")
    print(df.describe(include=[np.number])) # For numerical columns
    print(df.describe(include=['O'])) # For object (string) columns
    print("DATA TYPES")
    df.info()
    print("UNIQUE DATA")
    print(df.nunique())
    print("-------------------------\n")
```

This dataset is classified as longitudinal because it repeatedly captures data about individual users of a smart devices company over a two-month period. Longitudinal datasets like this are valuable for tracking changes and patterns over time, offering insights into user behavior and device usage trends.

The Fitbit Fitness Tracker dataset comprises files that record data at varying levels of granularity. One such file, `daily_activity_merged.ftr`, aggregates daily activity data and includes information previously found in `daily_calories_merged.ftr`, `daily_steps_merged.ftr`, and `daily_intensities_merged.ftr`. Due to the redundancy, the latter files will not be considered in further analysis. The `daily_activity_merged.ftr` file features 15 columns, such as `Id` for user identification, `ActivityDate` for the date of activity, and `TotalSteps`, which records the total number of steps taken by a user within a 24-hour period. It also includes columns for distances covered at various activity levels—`TotalDistance`, `TrackerDistance`, `VeryActiveDistance`, `ModeratelyActiveDistance`, `LightActiveDistance`, and `SedentaryActiveDistance`—and for minutes spent in different activity intensities—`VeryActiveMinutes`, `FairlyActiveMinutes`, `LightlyActiveMinutes`, and `SedentaryMinutes`. The `Calories` column notes the number of calories burned per day. Additionally, `sleep_day_merged.ftr` provides insights into sleep patterns with columns like `TotalSleepRecords`, `TotalMinutesAsleep`, and `TotalTimeInBed`. Lastly, `weightLogInfo_merged.ftr` contains eight columns related to users' daily weight measures.

Starting from the highest level of granularity, there are three files that represent data about intensities, steps, and calories. These files contain the same variables recorded at the daily level of granularity. Moving down to a lower level, there are eight files. Four of these contain the same data at the daily and hourly level. There is also one file that measures METs. The remaining files represent the data in a wide format that is not useful in our analysis. Lastly, at the second level of granularity, there is one file that records heart rate measurements every five seconds.

### Check data integrity

Determining data integrity is a critical process in data analysis to ensure that the data is both accurate and complete, and remains consistent throughout the analysis. In the following sections, I will demonstrate the application of various techniques designed to achieve this goal. These techniques aim to provide a thorough and effective means of managing and controlling the data during the analysis process.

#### Determine the statistical power of data

As previously mentioned, the sample size of the dataset is insufficient to produce reliable recommendations for Bellabeat's marketing team, particularly in the realm of prescriptive analytics. Consequently, any conclusions drawn from this analysis will be purely descriptive.

#### Overcome the challenges of insufficient data

No datasets fit enough to serve as proxy data were found. I check the most relevant websites like Kaggle, Google's Dataset Search, data.world, USA government open datasets, etc. Resulting in Apple Watch and Fitbit data. This dataset has a larger sample size of 46, but only tracks them for 65 minutes, making it useless to achieve the objectives.

#### Discover data constraints and clean data

Inappropiate data types were found in features representing dates at different levels of granularity. To solve this, all dates were standarized to a single format assigning the UTC time format.

```python
def convert_object_to_datetime(dfs):
  """
  Converts datetime-like strings in multiple DataFrames to actual datetime objects.

  Args:
    dfs (list[pd.DataFrame]): A list of pandas DataFrames to be processed.
  """

  for df in dfs.values():
    # Identify columns with object data type
    object_cols = df.select_dtypes(include=['object']).columns

    # Apply to_datetime() to columns, handling potential errors
    for col in object_cols:
      try:
        df[col] = pd.to_datetime(df[col], errors='coerce')
      except (ValueError, TypeError) as e:
        print(f"Error converting column '{col}': {e}")
convert_object_to_datetime(dataframes)
```
Data range for each column was verified for accuracy. Based on certain conditions like date range and ID length, etc., filters were applied [9-11]. Following this, we proceeded to reduce the data across various categories. Daily activity data was reduced from 940 observations to 936, representing a decrease of 0.42%. Sleep day data remains the same. Weight data was reduced from 67 observations to 28, a decrease of 58.20%. Hourly intensities data was reduced from 22099 observations to 21859, a decrease of 1.08%. Similar reductions occurred with hourly steps and calories. Minute intensities data was reduced from 1,325,580 observations to 1,310,419, a decrease of 1.14%. Almost identical reductions also occurred with minute steps data, minute METs data, minute calories data, and minute sleep data (1.53%). Heart rate data was reduced from 2,483,658 observations to 2,462,069, a decrease of 0.86%.

```python
# Filter daily activity data
daily_activity_corrected_data_ranges = dataframes['dailyActivity'][
    # Check if Id has a length of 10
    (dataframes['dailyActivity']['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityDate falls between a date range
    (dataframes['dailyActivity']["ActivityDate"] >= "2016-03-12") &
    (dataframes['dailyActivity']["ActivityDate"] <= "2016-05-12") &
    # Check if TotalSteps is greater or equal to 0
    (dataframes['dailyActivity']["TotalSteps"] >= 0) &
    # Check if TotalDistance is greater or equal to 0
    (dataframes['dailyActivity']["TotalDistance"] >= 0) &
    # Check if TrackerDistance is greater or equal to 0
    (dataframes['dailyActivity']["TrackerDistance"] >= 0) &
    # Check if LoggedActivitiesDistance is greater or equal to 0
    (dataframes['dailyActivity']["LoggedActivitiesDistance"] >= 0) &
    # Check if VeryActiveDistance is greater or equal to 0
    (dataframes['dailyActivity']["VeryActiveDistance"] >= 0) &
    # Check if ModeratelyActiveDistance is greater or equal to 0
    (dataframes['dailyActivity']["ModeratelyActiveDistance"] >= 0) &
    # Check if LightActiveDistance is greater or equal to 0
    (dataframes['dailyActivity']["LightActiveDistance"] >= 0) &
    # Check if SedentaryActiveDistance is greater or equal to 0
    (dataframes['dailyActivity']["SedentaryActiveDistance"] >= 0) &
    # Check if VeryActiveMinutes is greater or equal to 0
    # but less or equal to 1440 min (24 h)
    (dataframes['dailyActivity']["VeryActiveMinutes"] >= 0) &
    (dataframes['dailyActivity']["VeryActiveMinutes"] <= 1440) &
    # Check if FairlyActiveMinutes is greater or equal to 0
    # but less or equal to 1440 min (24 h)
    (dataframes['dailyActivity']["FairlyActiveMinutes"] >= 0) &
    (dataframes['dailyActivity']["FairlyActiveMinutes"] <= 1440) &
    # Check if LightlyActiveMinutes is greater or equal to 0
    # but less or equal to 1440 min (24 h)
    (dataframes['dailyActivity']["LightlyActiveMinutes"] >= 0) &
    (dataframes['dailyActivity']["LightlyActiveMinutes"] <= 1440) &
    # Check if SedentaryMinutes is greater or equal to 0
    # but less or equal to 1440 min (24 h)
    (dataframes['dailyActivity']["SedentaryMinutes"] >= 0) &
    # Check if calories are greater than 0
    (dataframes['dailyActivity']["Calories"] > 0)
    ]
print("Observations after filtering of data:", daily_activity_corrected_data_ranges.shape[0])

# Filter sleep day data
sleep_day_corrected_data_ranges = dataframes["sleepDay"][
    # Check if Id has a length of 10
    (dataframes["sleepDay"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if "SleepDay" falls between a date range
    (dataframes["sleepDay"]["SleepDay"] >= "2016-03-12") &
    (dataframes["sleepDay"]["SleepDay"] <= "2016-05-12") &
    # Check if recorded sleep periods are greater than 0
    (dataframes["sleepDay"]["TotalSleepRecords"] > 0) &
    # Check if the amount of minutes classified as
    # being asleep falls between a time range
    (dataframes["sleepDay"]["TotalMinutesAsleep"] >= 0) &
    (dataframes["sleepDay"]["TotalMinutesAsleep"] <= 1440) &
    # Check if the amount of minutes classified as
    # being asleep falls between a time range
    (dataframes["sleepDay"]["TotalTimeInBed"] >= 0) &
    (dataframes["sleepDay"]["TotalTimeInBed"] <= 1440)
    ]
print("Observations after filtering of data:", sleep_day_corrected_data_ranges.shape[0])

# Filter weight data under certain conditions
weight_log_info_corrected_data_ranges = dataframes["weightLogInfo"][
    # Check if Id has a length of 10
    (dataframes["weightLogInfo"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if "Date" falls between a date range
    (dataframes["weightLogInfo"]["Date"] >= "2016-03-12") &
    (dataframes["weightLogInfo"]["Date"] <= "2016-05-12") &
    # Check if WeightKg is greater than 0
    (dataframes["weightLogInfo"]["WeightKg"] > 0) &
    # Check if WeightPounds is greater than 0
    (dataframes["weightLogInfo"]["WeightPounds"] > 0) &
    # Check if Fat falls between established range
    (dataframes["weightLogInfo"]["Fat"] > 0) &
    (dataframes["weightLogInfo"]["Fat"] < 100) &
    # Check if BMI falls between an established range
    (dataframes["weightLogInfo"]["BMI"] > 0) &
    (dataframes["weightLogInfo"]["BMI"] < 100) &
    # Check if IsManualReport falls between a discrete established range
    (dataframes["weightLogInfo"]["IsManualReport"] == True) | (dataframes["weightLogInfo"]["IsManualReport"] == False) &
    # Check if LogId has a length of 13
    (dataframes["weightLogInfo"]['LogId'].astype(str).apply(lambda x: len(x) == 13))
    ]
print("Observations after filtering of data:", weight_log_info_corrected_data_ranges.shape[0])

# Filter hourly intensities data
hourly_intensities_corrected_data_ranges = dataframes["hourlyIntensities"][
    # Check if Id has a length of 10
    (dataframes["hourlyIntensities"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityHour falls between a date range
    (dataframes["hourlyIntensities"]["ActivityHour"] >= "2016-03-12") &
    (dataframes["hourlyIntensities"]["ActivityHour"] <= "2016-05-12") &
    # Check if TotalIntensity falls betweeen a range of values
    (dataframes["hourlyIntensities"]["TotalIntensity"] >= 0) &
    (dataframes["hourlyIntensities"]["TotalIntensity"] <= 180) &
    # Check if AverageIntensity falls between a range of values
    (dataframes["hourlyIntensities"]["AverageIntensity"] >= 0) &
    (dataframes["hourlyIntensities"]["AverageIntensity"] <= 3)
    ]

print("Observations after filtering of data:", hourly_intensities_corrected_data_ranges.shape[0])

# Filter hourly steps data
hourly_steps_corrected_data_ranges = dataframes["hourlySteps"][
    # Check if Id has a length of 10
    (dataframes["hourlySteps"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityHour falls between a date range
    (dataframes["hourlySteps"]["ActivityHour"] >= "2016-03-12") &
    (dataframes["hourlySteps"]["ActivityHour"] <= "2016-05-12") &
    # Check if StepTotal is greater than 0
    (dataframes["hourlySteps"]["StepTotal"] >= 0)
    ]
print("Observations after filtering of data:", hourlySteps_corrected_data_ranges.shape[0])

# Filter hourly calories data
hourly_calories_corrected_data_ranges = dataframes["hourlyCalories"][
    # Check if Id has a length of 10
    (dataframes["hourlyCalories"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityHour falls between a date range
    (dataframes["hourlyCalories"]["ActivityHour"] >= "2016-03-12") &
    (dataframes["hourlyCalories"]["ActivityHour"] <= "2016-05-12") &
    # Check if calories are greater than 0
    (dataframes["hourlyCalories"]["Calories"] > 0)
    ]
print("Observations after filtering of data:", hourly_calories_corrected_data_ranges.shape[0])

# Filter minute intensities data
minute_intensities_corrected_data_ranges = dataframes["minuteIntensitiesNarrow"][
    # Check if Id has a length of 10
    (dataframes["minuteIntensitiesNarrow"]["Id"].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityMinute falls between a date range
    (dataframes["minuteIntensitiesNarrow"]["ActivityMinute"] >= "2016-03-12") &
    (dataframes["minuteIntensitiesNarrow"]["ActivityMinute"] <= "2016-05-12") &
    # Check if Intensity falls between a discrete established range
    (dataframes["minuteIntensitiesNarrow"]["Intensity"] == 0) |
    (dataframes["minuteIntensitiesNarrow"]["Intensity"] == 1) |
    (dataframes["minuteIntensitiesNarrow"]["Intensity"] == 2) |
    (dataframes["minuteIntensitiesNarrow"]["Intensity"] == 3)
    ]
print("Observations after filtering of data:", minute_intensities_corrected_data_ranges.shape[0])

# Filter minute steps data
minute_steps_corrected_data_ranges = dataframes["minuteStepsNarrow"][
     # Check if Id has a length of 10
    (dataframes["minuteStepsNarrow"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityMinute falls between a date range
    (dataframes["minuteStepsNarrow"]["ActivityMinute"] >= "2016-03-12") &
    (dataframes["minuteStepsNarrow"]["ActivityMinute"] <= "2016-05-12") &
    # Check if Steps is greater than 0
    (dataframes["minuteStepsNarrow"]["Steps"] >= 0)
    ]
print("Observations after filtering of data:", minute_steps_corrected_data_ranges.shape[0])

# Downscale METs values by 10 (run once not multiple times)
dataframes["minuteMETsNarrow"]["METs"] = dataframes["minuteMETsNarrow"]["METs"] / 10

# Filter minute METs data
minute_METs_corrected_data_ranges = dataframes["minuteMETsNarrow"][
    # Check if Id has a length of 10
    (dataframes["minuteMETsNarrow"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityMinute falls between a date range
    (dataframes["minuteMETsNarrow"]["ActivityMinute"] >= "2016-03-12") &
    (dataframes["minuteMETsNarrow"]["ActivityMinute"] <= "2016-05-12") &
    # Check if METs is greater than 0.9
    (dataframes["minuteMETsNarrow"]["METs"] >= 0.9)
    ]
print("Observations after filtering of data:", minute_METs_corrected_data_ranges.shape[0])

# Filter minute calories data
minute_calories_corrected_data_ranges = dataframes["minuteCaloriesNarrow"][
    # Check if Id has a length of 10
    (dataframes["minuteCaloriesNarrow"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityMinute falls between a date range
    (dataframes["minuteCaloriesNarrow"]["ActivityMinute"] >= "2016-03-12") &
    (dataframes["minuteCaloriesNarrow"]["ActivityMinute"] <= "2016-05-12") &
    # Check if Calories are greater than 0
    (dataframes["minuteCaloriesNarrow"]["Calories"] > 0)
    ]
print("Observations after filtering of data:", minute_calories_corrected_data_ranges.shape[0])

# Filter minute sleep data
minute_sleep_corrected_data_ranges = dataframes["minuteSleep"][
    # Check if Id has a length of 10
    (dataframes["minuteSleep"]['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityMinute falls between a date range
    (dataframes["minuteSleep"]["date"] >= "2016-03-12") &
    (dataframes["minuteSleep"]["date"] <= "2016-05-12") &
    # Check if value falls between a discrete established range
    (dataframes["minuteSleep"]["value"] == 1) |
    (dataframes["minuteSleep"]["value"] == 2) |
    (dataframes["minuteSleep"]["value"] == 3) &
    # Check if logId has a length of 11
    (dataframes["minuteSleep"]['logId'].astype(str).apply(lambda x: len(x) == 11))
    ]
print("Observations after filtering of data:", minute_sleep_corrected_data_ranges.shape[0])

# Filter heartrate data (in seconds)
heartrate_seconds_corrected_data_ranges =  dataframes['heartrate_seconds'][
    # Check if Id has a length of 10
    (dataframes['heartrate_seconds']['Id'].astype(str).apply(lambda x: len(x) == 10)) &
    # Check if ActivityMinute falls between a date range
    (dataframes['heartrate_seconds']["Time"] >= "2016-03-12") &
    (dataframes['heartrate_seconds']["Time"] <= "2016-05-12") &
    # Check if Value is greater than 0
    (dataframes['heartrate_seconds']["Value"] > 0)
    ]
print("Observations after filtering of data:", heartrate_seconds_corrected_data_ranges.shape[0])
```
After removing duplicate data, some dataframes present no change in their size, those that do are sleep-related ones. The sleep data at the daily level of granularity decreased from 413 to 410 entries, a reduction of 0.72%. Similarly, at the minute level of detail, decreased from 185,619 to 185,076 records, a reduction of 0.29%.
```python
def drop_duplicates(df, desc):
    df_unique = df.drop_duplicates(keep='first')
    print(f"{desc} unique Ids:", df_unique["Id"].nunique())
    print(f"Observations before removal of duplicate {desc}:", df.shape[0])
    print(f"Observations after removal of duplicate {desc}:", df_unique.shape[0])
    return df_unique

# Apply the function to each DataFrame
daily_activity_unique = drop_duplicates(daily_activity_corrected_data_ranges, "daily_activity")

sleep_day_unique = drop_duplicates(sleep_day_corrected_data_ranges, "sleep_day")

weight_log_info_unique = drop_duplicates(weight_log_info_corrected_data_ranges, "weight_log_info")

hourly_intensities_unique = drop_duplicates(hourly_intensities_corrected_data_ranges, "hourly_intensities")

hourly_steps_unique = drop_duplicates(hourly_steps_corrected_data_ranges, "hourly_steps")

hourly_calories_unique = drop_duplicates(hourly_calories_corrected_data_ranges, "hourly_calories")

minute_intensities_unique = drop_duplicates(minute_intensities_corrected_data_ranges, "minute_intensities")

minute_steps_unique = drop_duplicates(minute_steps_corrected_data_ranges, "minute_steps")

minute_METs_unique = drop_duplicates(minute_METs_corrected_data_ranges, "minute_METs")

minute_calories_unique = drop_duplicates(minute_calories_corrected_data_ranges, "minute_calories")

minute_sleep_unique = drop_duplicates(minute_sleep_corrected_data_ranges, "minute_sleep")

heartrate_seconds_unique = drop_duplicates(heartrate_seconds_corrected_data_ranges, "heartrate_seconds")
```

Cross-validation only happens with data at the daily level of granularity. Reducing daily activity data means a decrease from 936 observations to 300 observations (67.94%). This lets us with 23 users instead of 33. The sleep data remains almost the same with 409 observations and same amount of users. And, thee weight data remains the same.

```python
# Cross-field validate daily activity data
daily_activity_cross_validated = daily_activity_unique[
    # The distance tracked by Fitbit device should be equal
    # to the sum of distance at different activity levels
    (daily_activity_unique["TrackerDistance"] == daily_activity_unique["VeryActiveDistance"] +
    daily_activity_unique["ModeratelyActiveDistance"] +
    daily_activity_unique["LightActiveDistance"] +
    daily_activity_unique["SedentaryActiveDistance"])
]
print("Observations after cross-validation of data:", daily_activity_cross_validated.shape[0])
print("Daily activity unique Ids:", daily_activity_cross_validated["Id"].nunique())

# Cross-field validate daily sleep data
sleep_day_cross_validated = sleep_day_unique[
   # TotalTimeInBed should be greater than TotalMinutesAsleep
   (sleep_day_unique["TotalTimeInBed"] > sleep_day_unique["TotalMinutesAsleep"])
]
print("Observations after cross-validation of data:", sleep_day_merged_cross_validated.shape[0])
print("Sleep day unique Ids:",sleep_day_merged_cross_validated["Id"].nunique())

# Cross-field validate weight data
weight_log_info_cross_validated = weight_log_info_unique[
    # WeightPounds is always greater than WeightKg
    (weight_log_info_unique["WeightPounds"] > weight_log_info_unique["WeightKg"])
]
print("Observations after cross-validation of data:", weight_log_info_cross_validated.shape[0])
print("Daily weight info unique Ids:", weight_log_info_cross_validated["Id"].nunique())
```

Across all the variables and files in the dataset, there are missing values only in the weight data. The missing data is present in 92.85% of the values in the Fat column. In this situation the right thing to do is remove the column [12-15].

It's important to note that there are data for all Fitbit users at the hourly and minute level of granularity, with the exception of sleep-related data which is available at the minute level of granularity for  n=24 users. At the daily level of granularity, there are data for  n=31 users for daily activity data,  n=24 for sleep-related data, and  n=5 for weight-related data. Additionally, at the second level of granularity, there are data for  n=14 users for heart-related data.

```python
def observed_and_null_values_count(df):
    # Calculate missing values count
    missing_values_count = df.isnull().sum()

    # Calculate observed values count
    observed_values_count = df.notnull().sum()

    # Create a DataFrame with the counts
    counts_df = pd.DataFrame({
        'feature': df.columns,
        'missing_values_count': missing_values_count,
        'observed_values_count': observed_values_count
    })

    # Calculate percentages
    counts_df['missing_percent'] = (counts_df['missing_values_count'] / counts_df[['missing_values_count', 'observed_values_count']].sum(axis = 1)) * 100
    counts_df['observed_percent'] = (counts_df['observed_values_count'] / counts_df[['missing_values_count', 'observed_values_count']].sum(axis = 1)) * 100

    return counts_df

# Dictionary to hold all dataframes
dfs = {
    'daily_activity': daily_activity_cross_validated,
    'sleep_day': sleep_day_cross_validated,
    'weight': weight_log_info_cross_validated,
    'hourly_intensities': hourly_intensities_unique,
    'hourly_steps': hourly_steps_unique,
    'hourly_calories': hourly_calories_unique,
    'minute_intensities': minute_intensities_unique,
    'minute_steps': minute_steps_unique,
    'minute_METs': minute_METs_unique,
    'minute_calories': minute_calories_unique,
    'minute_sleep': minute_sleep_unique,
    'heartrate_data': heartrate_seconds_unique
}

# Apply the function to all dataframes and store the results in a dictionary
missing_data_counts = {key: observed_and_null_values_count(value) for key, value in dfs.items()}
```
Finnally, I exported the data for further use in the following sections. The same format remains.

```python
# Define the directory path
directory_path = "/kaggle/working/cs2-Bellabit-process-step/"

def ensure_clean_directory(path):
    """
    Ensures the existence of a clean directory at the specified path.

    Args:
        path (str): The path to the directory to create or clean.

    Raises:
        OSError: If an error occurs while removing or creating the directory.
    """

    if os.path.exists(path):
        # Remove existing directory and its contents
        shutil.rmtree(path)

    # Create the directory, ignoring any errors if it already exists
    os.makedirs(path, exist_ok=True)

def write_feather_files(data_dict, base_path):
    """
    Writes multiple Feather files to a specified directory, using data from a dictionary.

    Args:
        data_dict (dict): A dictionary where keys are filenames (without extensions) and values are data to be saved as Feather files.
        base_path (str): The base path of the directory where the files will be written.

    Raises:
        OSError: If an error occurs while creating file paths or writing files.
        ImportError: If the feather library is not installed.
    """

    try:
        for filename, data in data_dict.items():
            file_path = os.path.join(base_path, f"{filename}.ftr")  # Construct full file path with .ftr extension
            feather.write_feather(data, file_path)  # Write data to Feather file
    except OSError as e:
        raise OSError(f"Error writing Feather files: {e}") from e
    except ImportError as e:
        raise ImportError("feather library is required for this function. Please install it using 'pip install feather-format'") from e

# Ensure the directory is clean
ensure_clean_directory(directory_path)

# Data to be exported in the feather format
data_to_export = {
    "daily_activity_processed": daily_activity_cross_validated,
    "sleep_day_processed": sleep_day_cross_validated,
    "weight_log_info_processed": weight_log_info_cross_validated,
    "hourly_intensities_processed": hourly_intensities_unique,
    "hourly_steps_processed": hourly_steps_unique,
    "hourly_calories_processed": hourly_calories_unique,
    "minute_intensities_processed": minute_intensities_unique,
    "minute_steps_processed": minute_steps_unique,
    "minute_METs_processed": minute_METs_unique,
    "minute_calories_processed": minute_calories_unique,
    "minute_sleep_processed": minute_sleep_unique,
    "heartrate_seconds_processed": heartrate_seconds_unique,
}

# Write all feather files to the directory
write_feather_files(data_to_export, directory_path)
```
