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
