+++
title = "Lesson 2"
subtitle = "Data Cleaning and Visualization in R"
date = 2025-02-02T03:03:29-05:00
draft = false
weight = 1
+++

# Data Cleaning and Visualization in R

## 📘 Overview

Data cleaning is a crucial first step in any data analysis project. It is estimated that **80% of a data scientist's work** involves data preparation, with around **60% of that time dedicated to cleaning and organizing data**.

In this lesson, we will use the **nycflights13::flights** dataset to learn practical data cleaning and visualization techniques. This dataset contains **over 300,000 flights** departing from New York City in 2013.

{{% notice info %}}
**Expected Duration:** 45 minutes  
This is an introductory lesson - no prior R knowledge required!

Required packages:
- nycflights13
- dplyr
- tidyr
- ggplot2
{{% /notice %}}

## 📦 Package Installation

First, let's install and load the required packages:

```r
# Install necessary packages
install.packages("nycflights13")
install.packages("dplyr")
install.packages("tidyr")
install.packages("ggplot2")

# Load libraries
library(nycflights13)
library(dplyr)
library(tidyr)
library(ggplot2)
```

## 📌 Dataset Overview

The `flights` dataset contains:

- **Date & Time:** `year`, `month`, `day`, `dep_time`, `sched_dep_time`
- **Delays:** `dep_delay`, `arr_delay`
- **Flight Details:** `carrier`, `flight`, `tailnum`
- **Route Info:** `origin`, `dest`, `distance`, `air_time`

```r
# Load and inspect the data
data("flights")
dim(flights)
head(flights)
```

## 🛠 Data Cleaning Steps

### 1. Handling Missing Values

```r
# Count NAs in delay columns
sum(is.na(flights$dep_delay))  # NAs in departure delays
sum(is.na(flights$arr_delay))  # NAs in arrival delays

# Option 1: Remove rows with missing values
flights_no_na <- na.omit(flights)

# Option 2: Replace NAs with zeros
flights_filled <- flights %>%
    mutate(dep_delay = replace_na(dep_delay, 0),
           arr_delay = replace_na(arr_delay, 0))
```

### 2. Removing Duplicates

```r
# Check for and remove any duplicates
nrow(flights)           # before
nrow(distinct(flights)) # after
```

### 3. Improving Data Format

```r
# Clean up the data structure
flights_clean <- flights %>%
    filter(!is.na(dep_time)) %>%
    left_join(airlines, by = "carrier") %>%
    rename(airline_name = name, 
           carrier_code = carrier) %>%
    mutate(
        origin = factor(origin),
        dest = factor(dest),
        carrier_code = factor(carrier_code)
    )
```

## 📊 Data Visualization

### 1. Departure vs. Arrival Delays

```r
ggplot(flights_clean, aes(x = dep_delay, y = arr_delay)) +
    geom_point(alpha = 0.2) +
    labs(title = "Departure vs. Arrival Delay",
         x = "Departure Delay (minutes)",
         y = "Arrival Delay (minutes)")
```

### 2. Delay Distribution

```r
ggplot(flights_clean, aes(x = dep_delay)) +
    geom_histogram(binwidth = 15, fill = "skyblue", color = "black") +
    labs(title = "Distribution of Departure Delays",
         x = "Departure Delay (minutes)",
         y = "Number of Flights")
```

### 3. Delays by Airport

```r
ggplot(flights_clean, aes(x = origin, y = arr_delay)) +
    geom_boxplot(fill = "orange") +
    labs(title = "Arrival Delays by Origin Airport",
         x = "Origin Airport",
         y = "Arrival Delay (minutes)")
```

### 4. Monthly Flight Patterns

```r
flights_per_month <- flights_clean %>%
    count(month)

ggplot(flights_per_month, aes(x = month, y = n)) +
    geom_line(color = "blue") +
    geom_point() +
    labs(title = "Flights per Month (2013)",
         x = "Month",
         y = "Number of Flights")
```

## ✅ Key Takeaways

- Learned to identify and handle missing values using `na.omit()` and `replace_na()`
- Checked for duplicate records using `distinct()`
- Improved data structure with `rename()` and `mutate()`
- Created various visualizations to explore the data using `ggplot2`

## 🚀 Practice Exercise

Try the code yourself in our interactive notebook:

{{< button href="https://colab.research.google.com/drive/12xj0xSpRqXWgPXO4iTe3-MrSAu7IeF6T#forceEdit=true&sandboxMode=true" >}}Open in Google Colab{{< /button >}}

{{% notice info %}}
**Important:** After clicking the link above, click the "Copy to Drive" button in Colab to create your own editable copy of the notebook.
{{% /notice %}}