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

## Ask
Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand the answer the following questions:

How do annual members and casual riders use Cyclistic bikes differently? Why would casual riders buy Cyclistic annual memberships? How can Cyclistic use digital media to inﬂuence casual riders to become members? Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.


## Prepare
In this phase, I gather the data, describe it, ensure it has the correct format, credibility, and understand its limitations. I use the historical trip data from Cyclistic (a ficticious bike share company),using real data from the company Divvy, for the previous 12 months to tackle this problem [6]. The data was in csv format, which has reading and writing performance limitations. With that in mind, I adopted the rds format. The data is credible, so the insights obtained will be of a prescriptive nature.


### Describe data
The Cyclistic historical trip data records trips performed per month. Each trip is anonymized and includes trip start day and time, trip end day and time, trip start station, trip end station, and rider type. Cyclistic processed the data to remove trips that are taken by staff as they service and inspect the system; and any trips below 60 seconds in length (potentially false starts or users trying to re-dock a bike to ensure it was secure).

The data provided only covers the last 12 months, not representative of the entire population of Cyclistic users and their past behavior. Therefore, it can be considered a sample. So, the insights derived from this data can only be functional under the specific circumstances of the proposed business task.


### Move and format data

The rds file storage format, has become a popular choice for data storage due to its demonstrated benefits of speed and file size compared to the csv format as discussed in [7-8]. Finally, we save them as a rds file in a separate directory (`cs1-cyclistic-prepare-phase/`), implemented in the `convert_csv_to_rds` function.

```r
convert_csv_to_rds <- function(csv_files, output_filepath) {
  # Initialize an empty data frame
  combined_data <- data.frame()

  # Loop over each CSV file
  for (file in csv_files) {
    # Read the CSV file
    data <- readr::read_csv(file,
      na = c(
        naniar::common_na_strings,
        naniar::common_na_numbers,
        "",
        " "
      ),
      skip_empty_rows = TRUE
    )

    # Concatenate the data to the combined data frame
    combined_data <- rbind(combined_data, data)
  }

  # Create output directory if it doesn't exists
  if (!file_test("-d", output_filepath)) {
    dir.create(file.path(dirname(output_filepath)), recursive = TRUE)
  }

  # Save the combined data frame as an RDS file
  saveRDS(combined_data, file = output_filepath)

  cat("The files located at", dirname(csv_files)[1], "were merged and converted successfully to rds format")
  invisible(gc())
}

# Get a list of all files in the directory with the specified format
csv_files <- list.files("/kaggle/input/divvy-trip-data-03-2023-to-02-2024/",
  pattern = "*.csv",
  full.names = TRUE
)

# Output RDS file path
output_filepath <- "/kaggle/working/cyclistic-divvy-cs-prepare-phase/cyclistic-divvy-202303-2024-04.rds"

# Call the function
convert_csv_to_rds(csv_files, output_filepath)
```

### Check data credibility

The credibility of our data can be determined using the ROCCC system proposed in the certification:

The data is reliable because it was collected by Divvy. The data is original, because it was collected by Divvy. The data is comprehensive because it contains all information needed to answers the guiding questions to produce insights that drives the decision making of the stakeholders. The data is current, because the data has a timeframe that covers the last 12 months (1 year). The data is cited because the data was obtained from the original source.


### Understand data limitations

The data possess two main limitations, unknown population size and generalizability, making it difficult to draw conclusions about the entire population. However, the data still holds value for gaining insights under the specific circumstances imposed by the business task.
