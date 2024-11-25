+++
title = 'Exploratory Steps With Synthetic Data'
date = 2024-11-25T13:09:06-05:00
draft = false
+++

# Lesson: Introduction to R Programming and Data Science

## Overview
In this lesson, we will introduce you to R programming by loading a synthetic dataset, performing basic correlations, and visualizing the data with several plots. After each code block, we'll explain how the code works step by step.

### Prerequisites
Before we begin, make sure that you have R installed. You can download R from [CRAN](https://cran.r-project.org/) and use RStudio as an IDE to write and execute R code.

---

## Step 1: Loading a Synthetic Dataset

First, we'll create a synthetic dataset that simulates a simple survey of people's ages, heights, weights, and annual income. We will store this data in a data frame, which is a fundamental data structure in R.

```r
# Load the necessary library
library(tibble)

# Create a synthetic dataset
set.seed(123)  # Set a seed for reproducibility

data <- tibble(
  # Random ages between 20 and 60
  Age = sample(20:60, 100, replace = TRUE),  
  # Heights with a mean of 170 cm and SD of 10
  Height = rnorm(100, mean = 170, sd = 10),  
  # Weights with a mean of 70 kg and SD of 15
  Weight = rnorm(100, mean = 70, sd = 15),   
  # Income with a mean of 50,000 and SD of 15,000
  Income = rnorm(100, mean = 50000, sd = 15000)
)

# View the first few rows of the data
head(data)
```

### Explanation:
- **`library(tibble)`**: This loads the `tibble` package, which provides a modern version of data frames that are more user-friendly.
- **`set.seed(123)`**: Ensures that the random numbers generated are the same every time you run the code. This makes your results reproducible.
- **`tibble()`**: Creates a tibble (data frame) with columns for Age, Height, Weight, and Income.
- **`rnorm()`**: Generates random numbers from a normal distribution. We use this for Height, Weight, and Income.
- **`head(data)`**: Displays the first six rows of the dataset so we can inspect it.

---

## Step 2: Calculating Basic Correlations

Now, let's calculate the correlation between different columns (variables) in the dataset. Correlation tells us the strength and direction of the relationship between two variables.

```r
# Calculate correlations between numeric columns
correlation_matrix <- cor(
    data[, c(
        "Age",
        "Height",
        "Weight",
        "Income")])

# View the correlation matrix
print(correlation_matrix)
```

### Explanation:
- **`cor()`**: This function computes the correlation matrix for the selected columns in the dataset. It ranges from -1 to 1, where:
  - **1** indicates a perfect positive correlation.
  - **-1** indicates a perfect negative correlation.
  - **0** indicates no correlation.
- **`data[, c("Age", "Height", "Weight", "Income")]`**: This selects only the numeric columns from the dataset for the correlation computation.
- **`print(correlation_matrix)`**: Displays the resulting correlation matrix.

---

## Step 3: Creating Plots to Visualize the Data

Visualization is key in data science to explore relationships between variables. We will now generate a few plots.

### 1. Scatter Plot: Age vs. Income

```r
# Load the ggplot2 library for data visualization
library(ggplot2)

# Create a scatter plot of Age vs. Income
ggplot(data, aes(
    x = Age,
    y = Income)) +
  geom_point() +
  labs(
    title = "Scatter Plot of Age vs. Income",
    x = "Age",
    y = "Income")
```

### Explanation:
- **`library(ggplot2)`**: Loads the `ggplot2` package, which is a popular library for creating visualizations.
- **`ggplot(data, aes(x = Age, y = Income))`**: Initializes the plot with the `data` and sets the x and y axes to Age and Income, respectively.
- **`geom_point()`**: Adds points to the plot, creating a scatter plot.
- **`labs()`**: Adds a title and labels to the axes.

### 2. Histogram: Distribution of Height

```r
# Create a histogram of Height
ggplot(data, aes(x = Height)) +
  geom_histogram(
    binwidth = 2,
    fill = "blue",
    color = "black",
    alpha = 0.7) +
  labs(
    title = "Histogram of Height",
    x = "Height (cm)",
    y = "Frequency")
```

### Explanation:
- **`geom_histogram()`**: Creates a histogram, which shows the frequency distribution of the `Height` variable. We define the bin width as 2.
- **`fill`** and **`color`**: Customize the color of the bars and their borders.
- **`alpha`**: Adjusts the transparency of the bars.

### 3. Boxplot: Weight Distribution by Age Group

```r
# Create a boxplot of Weight by Age Group
data$AgeGroup <- 
    ifelse(
        data$Age < 30, 
        "Under 30", 
    ifelse(
        data$Age <= 40, 
        "30-40", 
        "Over 40"))
ggplot(
    data,
    aes(
        x = AgeGroup,
        y = Weight,
        fill = AgeGroup)
        ) +
  geom_boxplot() +
  labs(
    title = "Boxplot of Weight by Age Group",
    x = "Age Group",
    y = "Weight (kg)") +
  theme_minimal()
```

### Explanation:
- **`ifelse()`**: Creates a new variable, `AgeGroup`, which categorizes the participants into three age groups: "Under 30", "30-40", and "Over 40".
- **`geom_boxplot()`**: Creates a boxplot, which shows the distribution of Weight for each Age Group.
- **`theme_minimal()`**: Applies a clean minimal theme to the plot.

---

## Step 4: Summarizing the Results

We have completed a few basic tasks:
1. We loaded a synthetic dataset and explored its structure.
2. We calculated correlations between the numeric variables to understand their relationships.
3. We created visualizations using `ggplot2` to explore the data further.

The following key observations could be made:
- If the correlation between variables like Age and Income is strong, the scatter plot would show a clear trend.
- The histogram of Height helps us understand the distribution of people's heights in the dataset.
- The boxplot allows us to see how Weight varies across different age groups.

---

## Conclusion

In this lesson, you learned how to:
- Load and explore a synthetic dataset.
- Perform basic correlation analysis to understand relationships between variables.
- Visualize data using scatter plots, histograms, and boxplots in R.
