---
title: 'Cyclistic Divvy bike share company case study'
date: 2024-04-03T09:08:29-06:00
draft: false
author: Christian Montenegro
tags:
  - R
  - Data transformation
  - Data cleaning
  - Data analysis
  - Data visualization
  - Data storytelling
image: 'bicycle-sharing-systems_sweden.jpg'
mathjax: true
---

## Summary

### Goal

The objective of this project is to use the data analytics life cycle to solve the business task of Cyclistic, a fictional bike-sharing company. I use the Divvy historical trip data to support my case. My insights will help to design a marketing campaign that aims to encourage casual users (single or day pass) to become annual subscribers.


### Process

I begin the data analytic process by obtaining the questions that will drive my analysis. Once I have identified these questions, I retrieve the necessary data from Divvy and merge them into a single feather-formatted file. Before proceeding with the analysis, I verify the data integrity and clean data where necessary. On the analysis phase, I perform data transformation to extract meaningful insights. Once I have identified the key findings, I share my conclusions. Finally, I present my recommendations to drive decision-making.



### Highlights

This project is carried out using R, using data-science related packages given its capabilities to handle medium to large sized datasets.

Casual and member users show clear patterns in different travel-related metrics. Casual users tend to travel farther than member users regardless of time periods. The same happens when comparing trip length. This pattern changes in bike rides performed where member users performed most of them regardless of time periods.

## Introduction

The market for bike-sharing services in the United States is expanding [1], but it also faces challenges such as operational inefficiencies, a single profit model, and issues with station congestion [2-3]. A variety of factors influence this expansion,ranging from user demographics to environmental considerations and market dynamics [4]. By 2024, it's to reach a revenue of US$320.30 millions, with 96% of the total revenue generated through online sales by 2028, and the number of users expected to amount to 4.70 million users in the same year [1]. To control these challenges, it has been suggested to diversify profit sources, optimize regional distribution, and enhance marketing strategies. Cyclistic can improve marketing strategies using data, while keeping in mind the primary motivators for usage such as convenience, perceived value, and economic considerations [5] without neglecting other factors mentioned above.

## Background

In 2016, Cyclistic launched a successful bike-share oﬀering. Since then, the program has grown to a ﬂeet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

## Business task

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the ﬂexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s ﬁnance analysts have concluded that annual members are much more proﬁtable than casual riders. Although the pricing ﬂexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.


## Scenario
This capstone project involves a junior data analyst working in the marketing analyst team at Cyclistic. To provide insights that will drive business-decision making, I will use the data analysis process proposed in the Google Data Analytics Professional Certificate, containing six stages: Ask, Prepare, Process, Analyze, Share, and Act. The stages define how data is generated, collected, processed, used, and analyzed to achieve business goals.

## Load required libraries
```r
# List of necessary packages
packages <- c(
    "ggplot2",
    "devtools",
    "tidyverse",
    "ggrepel",
    "naniar",
    "mice",
    "sf",
    "sp",
    "numform",
    "data.table",
    "geosphere",
    "stats",
    "gridExtra",
    "cowplot",
    "gtable",
    "moments",
    "patchwork"
)

# Check and install missing packages in the current environment
installed_packages <- packages %in% rownames(installed.packages())
if (any(installed_packages == FALSE)) {
  install.packages(packages[!installed_packages])
}

# Load all packages
invisible(lapply(packages, library, character.only = TRUE))
```
