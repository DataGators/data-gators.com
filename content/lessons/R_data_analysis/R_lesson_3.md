+++
title = "R_Lesson_3"
subtitle = "Working with Iris Dataset: Visualizations, Correlations, Avona and more in R"
draft = false
+++

## 📘 Overview

Data pre-processing is an important part of any aspect of data analysis. Regardless of what field of study you may find yourself in, ensuring the validity of your data is paramount to establishing a credible basis to your work. Otherwise, it can lead to inaccurate results and provide you with the wrong information for your project or study.

<!-- [Link to the Colab Template](https://colab.research.google.com/drive/1sPNX9brpkIkzCnD7k24L7X1gcIgUmEpO?usp=sharing) -->

[Link to the Colab Template](https://colab.research.google.com/drive/1tQx-WCFtkweU2JpGZOC_qpLRIr1LXY_y?usp=drive_link)

**Expected Duration:** 45 minutes

This is an introductory lesson - no prior R knowledge required!
Required packages:
- datasets
- ggplot2

## 📦 Package Installation

In this lesson, we will be using the `datasets`, `ggplot2`, `corrplot`, and `car` packages for our code. Let's start!

```r
if (!require(datasets)) {
    # Install the 'datasets' package if it is not installed
    install.packages("datasets")
    # Load the 'datasets' package
    library(datasets)
}
if (!require(ggplot2)) {
    install.packages("ggplot2")
    library(ggplot2)
}
if(!require(car)){
  install.packages("car")
  library(car)
}
if(!require(corrplot)){
  install.packages("corrplot")
  library(corrplot)
}
```

**Explanation:**

- `if (!require(datasets))` checks to see if the `datasets` package is installed. The same goes for the same statement checking to see if `ggplot2` is installed.
- `install.packages()` is used to install the corresponding package that is included within the parentheses.
- `library()` is used to load the methods or functions within the specified package for use in our code. Without this command, we would not be able to use the methods contained in the package.

## 📌 Dataset Overview

For this lesson, let's work with the built-in `iris` dataset, which contains data on 50 iris flowers across 3 different species of iris flowers. The `iris` dataset is a product of a botanical study carried out in 1936 by Ronald Fisher called *The use of multiple measurements in taxonomic problems*. The data of this study we will be using contains variables on respective length and width of petals and sepals of iris flower.

Each entry contains the following measurements (as **averages**, except *species*) for the flowers:

- **Sepal length** - The length of the sepal leaves (in cm)
- **Sepal width** - The width of the sepal leaves (in cm)
- **Petal length** - The length of the petal (in cm)
- **Petal width** - The width of the petals (in cm)
- **Species** - The respective species of the flower

We will break down the basic steps that make up pre-processing the dataset. At the end, we will see the benefit and importance of having n adequately prepared dataset, and how failing to adequately do so can cause issues with our analyses.

## 🛠 Data Pre-Processing Steps

### Step 1: Load and Preview Data

We are going to load in the dataset, and then look over its structure and dimensions.\

```r
data(iris)

# Display the first few rows of the dataset
cat("\nFirst few rows of the iris dataset:\n")
print(head(iris))

# Display the structure of the dataset
cat("\nStructure of the iris dataset:\n")
str(iris)
```

**Explanation:**

- `data(iris)` loads in the `iris` dataset.
  
- `head(iris)` displays the first few rows of the dataset.
  
- `str(iris)` displays the structure of the dataset, including the number of observations (`n`) and variables (`length`).

- `cat()` is similar to the `print()` function but offers more flexibility in formatting and concatenating strings. It operates in a manner similar to an `f-string` in Python for those who are familiar with that concept.

***NOTE:*** If you look at the output, you will see that there is a data type associated with each of the variables as the first textual tag of each variable of the output of `str(iris)`. Take note that the data type of the *Species* variable is `Factor`. This data type is unique to R, and is specifically for categorical variables, in this case being the species of iris flower.

### Step 2: Evaluate the data

Now that we've got the dataset loaded, and can draw some basic information from it, let's make sure there's no missing or irregular values.

```r
cat("\nChecking for NA values in the dataset:\n")
print(any(is.na(iris)))

cat("\nChecking for duplicate rows in the dataset:\n")
print(any(duplicated(iris)))
print(sum(duplicated(iris)))

cat("\nBoxplot for Sepal.Length to check for outliers:\n")
boxplot(iris$Sepal.Length, main="Boxplot for Sepal Length", ylab="Sepal Length")

cat("\nLevels of the Species factor:\n")
print(levels(iris$Species))

cat("\nChecking for zero or negative values in Sepal.Length:\n")
print(any(iris$Sepal.Length <= 0))
```

**Explanation:**

- `is.na()` checks to see if any of the values within the dataset are missing. The use of `any()` serves as a wrapper to see if any of the values are missing, rather than for a specific value.

- `duplicated()` checks to see if any of the rows within the dataset are duplicated.

- `sum()` will calculate the sum of the inserted condition within the bounds of the specified object. In the example, we prompted for a sum of all applicable `duplicated()` objects within the dataset, and we were given a count of a singular duplicated entry within the dataset.

- `boxplot()` creates a boxplot of the `Sepal.Length` variable within the dataset given the argument of `iris$Sepal.Length`.

- `levels()` returns the levels of the `Species` variable within the dataset given the argument of `iris$Species`.

- `any(iris$Sepal.Length <= 0)` checks to see if any of the values within the `Sepal.Length` variable are zero or negative.

### Step 3: Cleaning and Preparing the Data

In this step, we'll take a look at some of the data cleaning techniques that you can use in R to prepare the data for analysis. We already know from the previous step that there is a duplicate row in the dataset, so let's remove it. Let's also check the dataset after it has been cleaned to make sure that the duplicate row has been removed, and there's no missing values beyond the one we removed.

```r
print("\nRemoving duplicate rows from the dataset:\n")
iris <- iris[-which(duplicated(iris)),]

str(iris)
```

**Explanation:**

- `which(duplicated(iris))` returns the indices of the duplicate rows within the dataset.
- `iris[-which(duplicated(iris)),]` removes the duplicate rows from the dataset.
- `str(iris)` prints the structure of the cleaned dataset, as we had done in step 2. We are doing this to look over the new structure of the dataset to ensure the only change made is removing the duplicated value.

### Step 4: Explorative Analysis

Now that we've eliminated the duplicate value, and have ensured that there are no other excess changes, let's take a look at some more actions we can take to analyze the dataset.

```r
# Split the iris dataset by species
iris_setosa <- iris[iris$Species == "setosa", ]
iris_versicolor <- iris[iris$Species == "versicolor", ]
iris_virginica <- iris[iris$Species == "virginica", ]

# Print the first few rows of each dataset
print(head(iris_setosa))
print(head(iris_versicolor))
print(head(iris_virginica))

# Print the number of species in each dataset to ensure they only contain one species
cat("Number of species in the setosa dataset:", length(unique(iris_setosa$Species)), "\n")
cat("Number of species in the versicolor dataset:", length(unique(iris_versicolor$Species)), "\n")
cat("Number of species in the virginica dataset:", length(unique(iris_virginica$Species)), "\n")
```

**Explanation:**

- `iris[iris$Species == "setosa", ]` creates a new dataset that contains all the data associated with the *setosa* species in the dataset. The same action is done for the *versicolor* and *virginica* datasets, respectively.
- `length(unique(iris_setosa$Species))` Will give us the amount of species within the `iris_setosa` dataset. We are doing this to make sure that there is only one species in the dataset, and that we properly executed the code.


Now that we've separated the datasets into their own respective partitions based on species, let's explore what else we can do with this data.
For the next part of our analysis, let's plot the sepal and petal dimensions for each species.

```r
# Create a scatterplot for setosa with a line of best fit
ggplot(iris_setosa, aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ggtitle("Sepal Width vs. Sepal Length - Setosa")

ggplot(iris_setosa, aes(x = Petal.Width, y = Petal.Length)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ggtitle("Petal Width vs. Petal Length - Setosa")


# Create a scatterplot for versicolor with a line of best fit
ggplot(iris_versicolor, aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ggtitle("Sepal Width vs. Sepal Length - Versicolor")

ggplot(iris_versicolor, aes(x = Petal.Width, y = Petal.Length)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ggtitle("Petal Width vs. Petal Length - Versicolor")



# Create a scatterplot for virginica with a line of best fit
ggplot(iris_virginica, aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ggtitle("Sepal Width vs. Sepal Length - Virginica")

ggplot(iris_virginica, aes(x = Petal.Width, y = Petal.Length)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  ggtitle("Petal Width vs. Petal Length - Virginica")
```

**Explanation:**

- `ggplot(iris_virginica, aes(x = Sepal.Width, y = Sepal.Length))` Is the primary argument that sets up the graph, with the `x` and `y` variables being just those exactly. They correspond to the x and y dimensions of a traditional graph, and the variables that will be plotted along those dimensions.
- `geom_point()` establishes that the graph plotted will be a scatter plot.
- `geom_smooth(method = "lm", se = FALSE)`
    - `geom_smooth()`: Adds a smoothed conditional mean to a plot. A conditional mean is the average value of a variable within a specific group or subgroup defined by certain conditions. In the context of the code provided, it's essentially the average value of either sepal width/length or petal width/length for each individual species of iris flower.
    - `method = "lm"`: Specifies that a linear model (ordinary least squares regression) should be used to fit the smoothed line. This means it tries to find the "best fit" straight line through the data points.
    - `se = FALSE`: This removes the confidence interval around the smoothed line. The confidence interval (standard error) visually shows the uncertainty in the estimated relationship. By setting `se = FALSE`, we get a cleaner and less cluttered plot with only the fitted line.
 
## Part 2: Exploration with Comprehensive Data
In this next part of the lesson, we will be looking at a new dataset, this time with a more robust and comprehensive set of variables and information.

<p><a href="https://data.cityofnewyork.us/Environment/Air-Quality/c3uy-2p5r/about_data" target="_blank">Click here to check out the datset we'll be working with next.</a></p></div>

![image](https://github.com/user-attachments/assets/7622a8e0-9f49-4720-b7c5-a9b77451cadb)

If you get set to a redirect page like the one above, click the first link to continue to the landing page for the dataset we are working with. This is simply a redirect page, you haven't made a mistake.

### Step 1: Loading the Dataset

Now that you've taken a brief look at the data, let's take the first steps to working with it.

```r
nyc_airq <- read.csv(url("https://data.cityofnewyork.us/resource/c3uy-2p5r.csv"))
```

**Explanation**
- `nyc_airq <-` creates a new object with the name *nyc_airq*. `<-` act as an assignor, giving the *nyc_airq* object a value of the dataset we are reading in from a url. If you have worked with other languages in the past, it is similar to the `=` assignment operator. However, both Exist in R, and they have different meanings.
    - When using `=` inside a function call, the assignment is local to the function and does not affect the global environment. For example, `mean(x = 1:10)` calculates the mean of the vector 1:10 but does not create a variable x in the global environment.
    - When using `<-` inside a function call, the assignment is made to the global environment. For example, `mean(x <- 1:10)` creates a variable x in the global environment with the value `1:10`.
    - The `<-` operator has a higher precedence than `=`. This means that in expressions where both operators are used, `<-` will be evaluated first. For example, `x = y <- 5` is interpreted as `x = (y <- 5`), which assigns 5 to y and then assigns y to x.
- `read.csv(url("https://data.cityofnewyork.us/resource/c3uy-2p5r.csv")` creates a csv from a url object, in this case it is our dataset for our next activity. If you happen to use this function in the future, you have to make sure that the url leads to a *CSV* (Comma Separated Variable) file type format.

### Step 2: Reviewing the Dataset

Now that we've loaded up the dataset, let's take a look at it in the same way we did with the `iris` dataset.

```r
# Display the first few rows of the dataset
cat("\nFirst few rows of the NYC Air Quality dataset:\n")
print(head(nyc_airq))

# Display the structure of the dataset
cat("\nStructure of the NYC Air Quality dataset:\n")
str(nyc_airq)
```

One of the first things we see from this dataset is a new data type `character`.
- This data type is used to store alphanumeric data, such as names, addresses, descriptions, and more.
- `character` strings are always enclosed in either single (' ') or double (" ") quotes.
- They can include letters, numbers, symbols, spaces, and special characters.
- R will not perform mathematical operations or numerical comparisons on `character` variables or data unless it is explicitly converted to a numerical type.
- `character` data is useful for storing data that represents labels, descriptions, or categories.

To begin the necessary steps for analysis, we are going to look for duplicate and invalid (NA) values.

```r
# Checking the dimension of the dataset
cat("\nDimension of the NYC Air Quality dataset:\n")
dim(nyc_airq)

# Check for NA values
cat("\nNA values in the NYC Air Quality dataset:\n")
print(colSums(is.na(nyc_airq)))

# Check for duplicate rows
cat("\nDuplicate rows in the NYC Air Quality dataset:\n")
print(sum(duplicated(nyc_airq)))
```

**Explanation**
- `colSums(is.na(nyc_airq))` returns a sum of the amount per column of `na` or invalid values found in the dataset.
- `dim(nyc_airq)` returns the dimensions of the *nyc_airq* dataset.

### Step 3: Pre-Processing the Dataset

After checking for invalid and duplicate values in the dataset, we see that the `message` variable has 1000 invalid values, meaning that the whole of the data within this row is invalid. We can remove this column outright from the dataset in a single line of code!

```r
# Remove the "message" column
nyc_airq <- nyc_airq[, !names(nyc_airq) %in% "message"]

# Check the dataset to see if the column still exists.
View(nyc_airq$message)

# Check the dataset
head(nyc_airq)
```

**Explanation**

- `nyc_airq <-` assigns the result of the operation to the `nyc_airq` object, effectively updating the dataset.
- `nyc_airq[, ...]` selects specific columns from the `nyc_airq` data frame.
- `!names(nyc_airq) %in% "message"` creates a logical vector.
    - `names(nyc_airq)` extracts the names of the columns in the data frame.
    - `%in% "message"` checks if each column name is equal to "message".
    - `!` negates the result, so it becomes TRUE for all columns EXCEPT "message".
- In essence, this code removes the "message" column from the `nyc_airq` data frame by selecting all columns that are NOT named "message".

### Step 4: Explorative Analysis

Now that we've removed invalid data from our dataset, let's take a look at the correlation of the data!

```r
# Convert character variables to numeric (if possible)
nyc_airq_numeric <- nyc_airq
for (i in names(nyc_airq_numeric)) {
  if (is.character(nyc_airq_numeric[[i]])) {
    nyc_airq_numeric[[i]] <- as.numeric(as.factor(nyc_airq_numeric[[i]]))
  }
}

# Calculate the correlation matrix
correlation_matrix <- cor(nyc_airq_numeric, use = "pairwise.complete.obs")

# Create the correlation plot
corrplot(correlation_matrix, method = "circle", type = "upper")
```

**Explanation**

- Since the correlation plot is more technically involved, the explanation is longer to ensure you are adequately informed of what is going on during this step.

1. Convert Character Variables to Numeric (if possible):

   - The code iterates through each column of your data `nyc_airq_numeric`.
   - For each column that contains character data, it attempts to convert the data to numerical values.
   - `as.factor` converts the character data into factor levels (essentially assigns numerical representations to distinct values).
   - `as.numeric` then converts these factor levels into numeric values.

2. Calculate the Correlation Matrix:

   - `cor(nyc_airq_numeric, use = "pairwise.complete.obs")`:
     - This is the core part of the correlation analysis.
     - `cor` calculates the pairwise correlation coefficients between all numerical columns in the `nyc_airq_numeric` dataset.
     - `use = "pairwise.complete.obs"` specifies that only pairwise complete observations should be used for correlation calculations. This means that if there are missing values (NAs) in some rows/columns, it uses only the available data for each correlation pair.

3. Create the Correlation Plot:

   - `corrplot(correlation_matrix, method = "circle", type = "upper")`:
     - This line uses the `corrplot` library to create a visualization of the `correlation_matrix`.
     - `method = "circle"` indicates that the correlation coefficients should be represented using circles. The size of the circle corresponds to the magnitude of the correlation.
     - `type = "upper"` specifies that only the upper triangle of the correlation matrix should be plotted (since the lower triangle is a mirror image). This is typically done to avoid redundancy and make the plot easier to read.

In essence, the code performs the following steps:

1. **Prepares the data:** Converts character variables to numerical ones (if they can be reasonably converted) so that correlations can be calculated.
2. **Calculates correlations:** Determines the correlation between each pair of numerical variables in your dataset.
3. **Visualizes correlations:** Creates a plot (using `corrplot`) to help you see the relationships between variables visually. Strong positive correlations appear as larger, filled circles; strong negative correlations are typically indicated by smaller circles with different colors or shades.

It seems that `time_period` has a negative correlation with `data_value` (the raw value of the specified amount of pollutant). Let's take a look at the `time_period` variable of the dataset to explore more about this relationship.

```r
# Check the number of unique entries with the time_period column
unique(nyc_airq$time_period)
```

**Explanation**

- `unique(nyc_airq$time_period)` will return the amount of unique values within the `time_period` column.

```
# Plotting the relations of pollutant type per respective time-frame
ggplot(nyc_airq, aes(x = time_period, fill = name)) +
  geom_bar() +
  labs(title = "Pollutant Levels by Time Period",
       x = "Time Period",
       y = "Count",
       fill = "Pollutant Name") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

**Explanation**
- `ggplot(nyc_airq, aes(x = time_period, fill = name))` establishes the ggplot2 plot we are making (ggplot2 is the library we are using to make this plot).
  - `nyc_airq` is the dataset being used to produce the graph.
  - `aes(x = time_period, fill = name)` defines the aesthetic properties of the plot.
  - `fill = name` sets the fill color of each respective section of the bar graph to each value with the `name` variable.
- `geom_bar()` sets the state of the plot as a bar graph.
- `labs(title = ..., x = ..., y = ..., fill = ...)` adds labels to the plot for better understanding:
  - `title` sets the plot title.
  - `x` sets the label for the x-axis.
  - `y` sets the label for the y-axis (usually the count of observations).
  - `fill` sets the legend label for the fill color.
- `theme(axis.text.x = element_text(angle = 45, hjust = 1))` adjusts the plot theme.
  - `axis.text.x` targets the x-axis text labels.
  - `element_text(angle = 45, hjust = 1)` rotates the x-axis labels by 45 degrees and aligns them to the right to prevent overlapping.
 
Now that we've plotted the relationships of pollutants across time-frames, we see that Summer 2021 appears to be an outlier. This might have negatively affected our previous correlation plot. Let's remove it from our data set and see what happens.

```r
# prompt: remove `Summer 2021` data from the dataset via grepl

# Remove rows where time_period contains "Summer 2021"
nyc_airq <- nyc_airq[!grepl("Summer 2021", nyc_airq$time_period), ]

# Checking for the presence of Summer 2021 time period within the dataset
sum(nyc_airq$time_period=="Summer 2021")
```

**Explanation**
- `nyc_airq$time_period`: This accesses the "time_period" column of the `nyc_airq` data frame.
- `"Summer 2021"`: This is the pattern we're searching for.
- `grepl`:  This is a function in R that performs pattern matching using regular expressions. It stands for "grep logical."
- `grepl` returns a logical vector (TRUE or FALSE) indicating whether the specified pattern is found in each element of the `nyc_airq$time_period` column.

**How It's Used to Remove the Outlier**

1. `nyc_airq <- nyc_airq[!grepl("Summer 2021", nyc_airq$time_period), ]`
   - `!grepl(...)`:  The `!` operator negates the result of `grepl`. This means it will exclude data that is within the parameters of the time frame of Summer 2021.

Now let's move on to creating another correlation plot just as we did previously. Soon we will be able to see how the correlations have changed after removing outlier data points.

```r
# Convert character variables to numeric (if possible)
nyc_airq_numeric <- nyc_airq
for (i in names(nyc_airq_numeric)) {
  if (is.character(nyc_airq_numeric[[i]])) {
    nyc_airq_numeric[[i]] <- as.numeric(as.factor(nyc_airq_numeric[[i]]))
  }
}

# Calculate the correlation matrix
correlation_matrix <- cor(nyc_airq_numeric, use = "pairwise.complete.obs")

# Create the correlation plot
corrplot::corrplot(correlation_matrix, method = "circle", type = "upper")
```

Now that we've removed an outlier, we see that the correlation gained a stronger negative relationship between the respective time period air pollution data was recorded, and the start of the study where the data was recorded.

### Step 5: Statistical Analysis

To really get an in-depth look at our data, let's look at a one-way, and a two-way ANOVA test. Don't worry, these are very simple means of statistical analysis once we break them down.

Two-Way ANOVA vs. One-Way ANOVA

One-Way ANOVA:
* Used when you have one independent variable (factor) with more than two levels or groups, and you want to see if there are statistically significant differences in the mean of a dependent variable across these groups.
* Example: You are testing the impact of three different types of fertilizer on plant growth (dependent variable). Fertilizer type is the independent variable with three levels.
* It answers the question: "Is there a significant difference in the mean of the dependent variable among the different levels of the independent variable?"

Two-Way ANOVA:
* Used when you have two or more independent variables (factors), and you want to investigate their individual and combined effects on a dependent variable.
* Example: You are testing the impact of both fertilizer type (factor 1) and soil pH (factor 2) on plant growth (dependent variable).
* It answers questions like:
    * "Is there a significant main effect of fertilizer type on plant growth?"
    * "Is there a significant main effect of soil pH on plant growth?"
    * "Is there a significant interaction effect between fertilizer type and soil pH on plant growth?"

Key Differences:
* Number of Independent Variables: One-Way ANOVA has only one independent variable, while Two-Way ANOVA has two or more.
* Interaction Effects: Two-Way ANOVA allows you to investigate interaction effects between independent variables. For instance, the effect of one independent variable might depend on the level of another independent variable. One-Way ANOVA does not consider interactions.
* Complexity: Two-Way ANOVA is more complex than One-Way ANOVA due to the presence of multiple factors and the possibility of interactions.

In summary, choose One-Way ANOVA when you have only one independent variable and want to see if there are differences in the mean of the dependent variable among its levels. Choose Two-Way ANOVA when you have two or more independent variables and want to investigate their individual and combined effects on the dependent variable, including potential interaction effects.

```r
# One-way ANOVA for data_value against time_period
oneway_time_period <- aov(data_value ~ time_period, data = nyc_airq)
summary(oneway_time_period)

# One-way ANOVA for data_value against geo_place_name
oneway_geo_place_name <- aov(data_value ~ geo_place_name, data = nyc_airq)
summary(oneway_geo_place_name)

# One-way ANOVA for data_value against name
oneway_name <- aov(data_value ~ name, data = nyc_airq)
summary(oneway_name)
```

```r
# Perform two-way ANOVA with interaction
anova_result <- aov(data_value ~ time_period+name+geo_place_name, data = nyc_airq)

# Print the ANOVA table
print(summary(anova_result))
```

**Explanation**
  
- The code performs a two-way ANOVA test to analyze the impact of multiple independent variables on a dependent variable.

1. **`anova_result <- aov(data_value ~ time_period+name+geo_place_name, data = nyc_airq)`:**

   - **`aov()`:** This is the function in R that performs the analysis of variance (ANOVA).
   - **`data_value ~ time_period+name+geo_place_name`:** This part defines the model.
     - `data_value` is the dependent variable (the variable we want to explain).
     - `time_period+name+geo_place_name` are the independent variables (the factors that we think might affect the dependent variable).
   - **`data = nyc_airq`:** Specifies that the data should be taken from the `nyc_airq` dataframe.

2. **`print(summary(anova_result))`:**

   - **`summary()`:**  This function provides a summary of the ANOVA results, including the F-statistic, p-values, and degrees of freedom.
   - `print()` displays this summary output to the console.


**In essence, this code is analyzing how the dependent variable `data_value` varies based on different levels of `time_period`, `name`, and `geo_place_name`. The ANOVA test helps to determine if these factors have a statistically significant impact on `data_value`.**

Now that we've gone through practicing a very substantial amount of data analysis skills, you can save this workshop as a copy on your Google Drive to look back at this for reference for future analysis, as well as to further play around in R and see what else you can do!

### Conclusion

In this lesson, we've gone through the basics of loading and exploring the `iris` dataset in R. We've also seen how to check for missing values, outliers, and duplicate rows in the dataset in the `nyc_airq` dataset along with the `iris` dataset too!

We've also seen how to use `ggplot2` to create visualizations of the data. It is an important practice to check your data once you've found a source that you're comfortable with, especially if you think it is reliable. Even small abnormalities within data can cause issues with your analyses, and might be hidden until you visualize the data and come across them. Being sure to check your data through visualizations and pre-processing is an important part of ensuring that your analyses are accurate and reliable.

Good work, you've learned the basics of data pre-processing in R!

### What's Next?

- You can try modifying the plots to explore other variables or try additional visualizations like histograms, density plots, or heatmaps.
- Experiment with different `ggplot2` functions for more advanced visualizations.
- Try out graphing specific attributes of the dataset, or create your own visualizations to illustrate interactions between variables.

This workshop covered the basics of data analysis via pre-processing, thanks for attending 😄!
