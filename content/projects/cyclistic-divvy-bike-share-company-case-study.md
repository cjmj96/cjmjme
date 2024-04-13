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


## Process

To make sure the data we're working with is accurate, I used certain tools and techniques to process it. First, I looked at each piece of data closely to understand its limitations and challenges. Then, I applied processes to clean the data and remove any errors or inconsistencies. By doing this, we can trust that our results are based on accurate information.


### Explore data

To understand the structure of the data, we can load the feather files that contains observations from two months. By examining this data, we can gain insights into the usage patterns of Cyclistic users and make informed decisions about its operation and improvement.
```r
# Load dataset
cdtd_process <- readRDS("/kaggle/working/cyclistic-divvy-cs-prepare-phase/cyclistic-divvy-202303-2024-04.rds")

# Get a summary of the data
sprintf("Summary statistics of Cyclistic trip data")
summary(cdtd_process)

# Check the dimensions of your data
sprintf("Dimensions of Cyclistic trip data")
dim(cdtd_process)

# Check the names of the features
sprintf("Names of features from Cyclistic trip data")
names(cdtd_process)

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```
The data is longitudinal due it contains information about different users over time (12 months). It can also considered transactional, as it captures individual transactions (rides) by distinct users, where each ride involves one or more items (e.g., start station, end station, bike type, etc.).

The data contains 13 columns that gives detailed information on bike rides. The columns can be divided in 3 groups by the content they contain. The first group contains columns identifying ride and bike type (`ride_id` and `bike_type`). The second group identifies when and where a ride started and ended in different information levels. The first level identifies when a user took a ride (`started_at`) and when it ended (`ended_at`). The second level reveals the start (`start_station_name`) and end station (`end_station_name`) a user took a bicycle to perform a ride. The third level uniquely identifies the start and end station for a ride. The fourth level geographically identifies where a ride begin (`start_lat`, `start_lng`) and where a ride ended (`end_lat`, `end_lng`). Finally, the third group only contains one column that indicates whether the rider was a member or a casual user (`member_casual`).

### 8.2 Check data integrity

Determining data integrity is a critical process in data analysis to ensure that the data is both accurate and complete, and remains consistent throughout the analysis. In the following sections, I will demonstrate the application of various techniques designed to achieve this goal. These techniques aim to provide a thorough and effective means of managing and controlling the data during the analysis process.


#### 8.2.1 Determine the statistical power of data, hypothesis testing, and margin of error

As previously mentioned, the sample size is unknown so the statistical power, the feasibility to perform hypothesis testing or report a margin of error is not possible. With this in mind I will only report data variability to provide some sense of uncertainty (spread of data, margin of error (indirectly), confidence intervals (indirectly)).


#### 8.2.2 Overcome the challenges of insufficient data

The data collected by Cyclistic is enough (~6,000,000 observations) to solve the business task.


#### 8.2.3 Discover data constraints and clean data

##### 8.2.3.1 Corroborate data types

In the process of data validation, we checked all features in the dataset to ensure they presented the correct data types. We expected a character data type in 7 out of 13 features, a numerical data type in 4 out of 13 features, and a POSIXct data type in 2 out of 13 features. This was performed by using str function, which listed all the features with their corresponding data types.

```r
str(cdtd_process)

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```
##### 8.2.3.2 Validate data ranges

After verifying data ranges for most columns, the dataset posess 4,260,116 observations, representing a 25.35% decrease. Only the columns depicting spatial coordinates were left intact because they will require a more complex procedure, explained in detail later.
```r
# Filter the data frame based on multiple conditions
cdtd_process_partially_filtered_data_ranges <- cdtd_process %>%
  filter(
    # Filter for classic_bike and electric_bike
    rideable_type %in% c("classic_bike", "electric_bike") &
    # Filter for rides starting between March 1, 2023 and February 29, 2024
    (started_at >= as.POSIXct("2023-03-01 00:00:00") & started_at <= as.POSIXct("2024-02-29 23:59:59")) &
    # Filter for rides ending between March 1, 2023 and March 1, 2024
    (ended_at >= as.POSIXct("2023-03-01 00:00:00") & ended_at <= as.POSIXct("2024-03-01 23:59:59")) &
    # Exclude rides starting at the HQ QR station
    start_station_name != "HQ QR" &
    # Exclude rides ending at the HQ QR station
    end_station_name != "HQ QR" &
    # Filter for member and casual riders
    member_casual %in% c("member", "casual")
  )
dim(cdtd_process_partially_filtered_data_ranges)
```

I verified the data ranges for columns depicting spatial coordinates using city boundaries from Chicago and Evanston, obtained from [9-10]. The data contains spatial information defining the boundaries of each city, including bounding box coordinates. This spatial information allows us to compare them against Cyclistic data. In that way, I confirmed whether a ride took place in Chicago or Evanston.

```r
# Set the chunk size
chunk_size <- 512

# Initialize empty lists to store the points
start_points_in_chicago <- list(nrow(cdtd_process_partially_filtered_data_ranges))
end_points_in_chicago <- list(nrow(cdtd_process_partially_filtered_data_ranges))
start_points_in_evanston <- list(nrow(cdtd_process_partially_filtered_data_ranges))
end_points_in_evanston <- list(nrow(cdtd_process_partially_filtered_data_ranges))

# Read cities boundaries shapefiles
chicago <- sf::st_read("/kaggle/input/chicago-city-boundaries-data/")
evanston <- sf::st_read("/kaggle/input/evanston-city-boundaries-data/")

# Transform the CRS of the spatial objects to a common CRS
chicago <- sf::st_transform(chicago, "+proj=longlat +datum=WGS84")
evanston <- sf::st_transform(evanston, "+proj=longlat +datum=WGS84")

# Set the number of iterations to print progress
n_iterations <- ceiling(nrow(cdtd_process_partially_filtered_data_ranges) / chunk_size)

# Loop through the data in chunks
for (i in seq(1, nrow(cdtd_process_partially_filtered_data_ranges), chunk_size)) {
  # Print information about the current chunk
  cat(sprintf("Processing chunk %d of %d (start = %d, end = %d)\n", ceiling(i / chunk_size),
    n_iterations, i, min(i + chunk_size - 1, nrow(cdtd_process_partially_filtered_data_ranges))))

  # Extract the current chunk of data
  chunk <- cdtd_process_partially_filtered_data_ranges[i:(min(i + chunk_size - 1, nrow(cdtd_process_partially_filtered_data_ranges))), ]

  # Convert the start and end points to sf objects
  start_points <- sf::st_as_sf(chunk %>% select("start_lng", "start_lat"), coords = c("start_lng", "start_lat"), na.fail = FALSE, crs = 4326)
  end_points <- sf::st_as_sf(chunk %>% select("end_lng", "end_lat"), coords = c("end_lng", "end_lat"), na.fail = FALSE, crs = 4326)

  # Determine the points that fall within each city
  # and store the results in their corresponding list
  # for the current chunk
  start_points_in_chicago[i:(min(i + chunk_size - 1, nrow(cdtd_process_partially_filtered_data_ranges)))] <- sf::st_within(start_points, chicago)
  end_points_in_chicago[i:(min(i + chunk_size - 1, nrow(cdtd_process_partially_filtered_data_ranges)))] <- sf::st_within(end_points, chicago)
  start_points_in_evanston[i:(min(i + chunk_size - 1, nrow(cdtd_process_partially_filtered_data_ranges)))] <- sf::st_within(start_points, evanston)
  end_points_in_evanston[i:(min(i + chunk_size - 1, nrow(cdtd_process_partially_filtered_data_ranges)))] <- sf::st_within(end_points, evanston)
}

# Create output directory if it doesn't exists
if (!file_test("-d", "/kaggle/working/cyclistic-divvy-cs-process-phase/")) {
  dir.create("/kaggle/working/cyclistic-divvy-cs-process-phase/", recursive = TRUE)
}

# Save data using saveRDS() function
saveRDS(start_points_in_chicago, file = "/kaggle/working/cyclistic-divvy-cs-process-phase/start_points_in_chicago.rds")
saveRDS(end_points_in_chicago, file = "/kaggle/working/cyclistic-divvy-cs-process-phase/end_points_in_chicago.rds")
saveRDS(start_points_in_evanston, file = "/kaggle/working/cyclistic-divvy-cs-process-phase/start_points_in_evanston.rds")
saveRDS(end_points_in_evanston, file = "/kaggle/working/cyclistic-divvy-cs-process-phase/end_points_in_evanston.rds")

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```
# mannually for freeing up memory resources
invisible(gc())
```

Upon loading the vector data into memory and incorporating it into the original dataset, I was able to streamline the computation process of the remaining features. This step effectively resolved previous computational issues, allowing for a problem-free calculation of these features.

```r
# Add ride duration
cdtd_analyze_sorted_filtered$ride_length <- difftime(
  cdtd_analyze_sorted_filtered$ended_at,
  cdtd_analyze_sorted_filtered$started_at,
  units = "secs") %>%
  as.character %>%
  as.numeric

# Create a feature representing the month of each ride
cdtd_analyze_sorted_filtered$month <- lubridate::month(
  cdtd_analyze_sorted_filtered$started_at,
  label = FALSE,
  abbr = FALSE
)

# Create a feature representing the day of each ride
cdtd_analyze_sorted_filtered$day_of_week <- lubridate::wday(
  cdtd_analyze_sorted_filtered$started_at,
  label= FALSE,
  abbr = FALSE,
  week_start = 7
)

# Create a feature representing the hour of each ride
cdtd_analyze_sorted_filtered$hour <- lubridate::hour(cdtd_analyze_sorted_filtered$started_at)


# Filter to make sure that ride_distance and ride_length
# have proper values
cdtd_analyze_sorted_filtered <- cdtd_analyze_sorted_filtered %>%
  filter(
    # Filter for ride distance greater than 0
    (ride_distance > 0) &
    # A ride should be greater than 0 if not it's
    # a false start
    (ride_length > 60)
  )

dim(cdtd_analyze_sorted_filtered)

# Show data set
head(cdtd_analyze_sorted_filtered)

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```

Before integrating the new data into the original dataset, I loaded it into memory. Once the data was accessible, I created two logical lists to represent the specified conditions. Finally, I incorporated the lists into the original dataset, significantly reducing the space complexity.

After filtering, the dataset posess 4,250,388 observations, representing a 0.22% decrease respect to the previous step.

```r
# load data using readRDS() function
start_points_in_chicago <- readRDS("/kaggle/working/cyclistic-divvy-cs-process-phase/start_points_in_chicago.rds")
start_points_in_evanston <- readRDS("/kaggle/working/cyclistic-divvy-cs-process-phase/start_points_in_evanston.rds")
end_points_in_chicago <- readRDS("/kaggle/working/cyclistic-divvy-cs-process-phase/end_points_in_chicago.rds")
end_points_in_evanston <- readRDS("/kaggle/working/cyclistic-divvy-cs-process-phase/end_points_in_evanston.rds")

# Create logical lists
in_chicago <- start_points_in_chicago == 1 & end_points_in_chicago == 1
in_evanston <- start_points_in_evanston == 1 & end_points_in_evanston == 1

# Add data in the original dataframe
cdtd_process_partially_filtered_data_ranges$in_chicago <- in_chicago
cdtd_process_partially_filtered_data_ranges$in_evanston <- in_evanston

# Filter the original dataframe based on the logical vectors:
cdtd_process_filtered_data_ranges <- cdtd_process_partially_filtered_data_ranges %>%
  filter(
    (cdtd_process_partially_filtered_data_ranges$in_chicago == TRUE) |
    (cdtd_process_partially_filtered_data_ranges$in_evanston == TRUE))

dim(cdtd_process_filtered_data_ranges)

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```
##### 8.2.3.3 Handle missing data

The presence of missing values in a dataset is universal. When handling inadequately lead to loss of eficiency and bias. First, The extension of information loss is intrinsically linked to the analysis question. Second, the subsets of complete observations may not be representative of the population under study. Restricting analysis to complete records may then lead to biased interpretations. The extent of such bias depends on the statistical behavior of the missing data. So, a formal framework to describe this behaviour is thus fundamental. I show what methodology was applied in the following sections.


###### 8.2.3.3.1 Amount of missing data
No missing values was found in the dataset. So we will follow with the next sections.
```r
missing_prop <- naniar::miss_var_summary(cdtd_process_filtered_data_ranges %>%
  # Select non city boundaries columns
  select(!c("in_chicago", "in_evanston")))
missing_prop
# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```

##### 8.2.3.4 Verify data uniqueness

There are no duplicate entries in the dataset. To do this, I looked at the `ride_id` feature, which uniquely identifies each ride in the dataset. I counted the number of unique `ride_id` values and compared it to the total number of observations in the dataset.
```r
# Leave only relevant features
cdtd_no_cities_boundaries_columns <- cdtd_process_filtered_data_ranges %>%
  # Select non city boundaries columns
  select(!c("in_chicago", "in_evanston"))

# Verify that ride_id has no duplicate data
n_univals_rid <- n_distinct(cdtd_no_cities_boundaries_columns$ride_id)
sprintf("Does Cyclistic trip data has unique data?: %s, because it possess %s different ride ids",
  n_univals_rid == nrow(cdtd_no_cities_boundaries_columns), n_univals_rid %>%
    prettyNum(., big.mark = ",", scientific = FALSE))

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```

##### 8.2.3.5 Check cross field conditions

After validating cross field conditions the dataset possess 3,988,632 observations, representing a 6.15% decrease.
```r
cdtd_cross_field_validated <- cdtd_no_cities_boundaries_columns %>%
  filter(
    # No rides can start and end at the same time
    (started_at != ended_at) &
    # No rides can have the same station_id at
    # the start and end of a ride
    (start_station_id != end_station_id) &
    # No rides can have the same station_name at
    # the start and end of a ride
    (start_station_name != end_station_name) &
    # No ride can have the same spatial coordinates
    # (latitude) at the start and end of
    # a ride
    (start_lat != end_lat) &
    # No ride can have the same spatial coordinates
    # (longitude) at the start and end of
    # a ride
    (start_lng != end_lng)
  )
dim(cdtd_cross_field_validated)

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```

##### 8.2.3.6 Removing extra spaces

The `str_squish` function was successfully used to remove extra spaces in features with the character data type. This function operates by identifying extra spaces present at the start of the text, in between words, and at the end of the text. It then replaces these extra spaces with a single space.

```r
# Remove extra spaces across features
cdtd_process_final <- cdtd_cross_field_validated %>%
  mutate(across(where(is.character), str_squish))

# Create output directory if it doesn't exists
if (!file_test("-d","/kaggle/working/cyclistic-divvy-cs-process-phase/")) {
  dir.create("/kaggle/working/cyclistic-divvy-cs-process-phase/", recursive = TRUE)
}

# Save data using saveRDS() function
saveRDS(cdtd_process_final, file = "/kaggle/working/cyclistic-divvy-cs-process-phase/cyclistic-divvy-202303-2024-04_processed.rds")

# Trigger garbage collection process
# mannually for freeing up memory resources
invisible(gc())
```
