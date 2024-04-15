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
