+++
title = 'Intro_mpg'
date = 2024-11-24T03:03:29-05:00
draft = false
+++

Let's explore the `mpg` dataset from the `ggplot2` package in R. This lesson will cover how to load the dataset, calculate basic correlations, and create various plots, with detailed explanations for each step.

---

## Introduction to R Programming with the MPG Dataset

In this lesson, we will work with the `mpg` dataset, which is included in the `ggplot2` package. The dataset contains information about the fuel efficiency (miles per gallon) of various car models, with additional details about each car such as:
- **mpg**: Miles per gallon (fuel efficiency).
- **cyl**: Number of cylinders in the car engine.
- **disp**: Displacement (engine size in cubic inches).
- **hp**: Horsepower.
- **drat**: Rear axle ratio.
- **wt**: Weight of the car (in 1000 pounds).
- **qsec**: Quarter mile time.
- **vs**: Engine type (0 = V-shaped, 1 = straight).
- **am**: Transmission type (0 = automatic, 1 = manual).
- **gear**: Number of forward gears.
- **carb**: Number of carburetors.

We will load the dataset, explore basic correlations, and visualize the data using plots.

---

### Step 1: Install and Load Required Libraries

To work with the `mpg` dataset, we first need to install and load the `ggplot2` package. If it's already installed, we can simply load it.

```r
# Install ggplot2 package if not already installed
# install.packages("ggplot2")

# Load the ggplot2 package
library(ggplot2)
```

**Explanation:**
- `install.packages("ggplot2")` installs the `ggplot2` package (if it hasn't been installed yet). `ggplot2` is a popular package for creating data visualizations in R.
- `library(ggplot2)` loads the package into R, so we can access its built-in datasets and plotting functions.

---

### Step 2: Load the MPG Dataset

The `mpg` dataset is included in `ggplot2` by default, so we can load it without any extra steps.

```r
# Load the mpg dataset
data("mpg")

# Check the first few rows of the dataset
head(mpg)
```

**Explanation:**
- `data("mpg")` loads the `mpg` dataset into memory.
- `head(mpg)` shows the first six rows of the dataset, so we can inspect the structure of the data.

**Output:**
```
  manufacturer model displ  year cyl trans drv cty hwy fl
1         audi  a4  1.8  1999   4   auto   f  18  29  2
2         audi  a4  1.8  1999   4   auto   f  21  29  2
3         audi  a4  2.0  1999   4   auto   f  21  29  2
4         audi  a4  2.0  1999   4   auto   f  19  27  2
5         audi  a4  2.8  1999   6   auto   f  16  26  3
6         audi  a4  2.8  1999   6   auto   f  18  26  3
```

**Explanation:**
- The dataset has columns for the manufacturer, model, engine displacement (`displ`), number of cylinders (`cyl`), transmission type (`trans`), drive type (`drv`), city and highway fuel efficiency (`cty`, `hwy`), and a factor variable `fl`.

---

### Step 3: Calculate Correlations

We can calculate the correlation between numeric variables such as `displ` (engine displacement), `cty` (city mileage), and `hwy` (highway mileage).

```r
# Calculate correlations
# between numeric variables (displ, cty, hwy)
cor(mpg[, c("displ", "cty", "hwy")])
```

**Explanation:**
- `mpg[, c("displ", "cty", "hwy")]` selects the numeric columns `displ`, `cty`, and `hwy` from the dataset.
- `cor()` calculates the correlation matrix, which shows the strength and direction of the linear relationships between these variables.

**Output:**
```
           displ       cty      hwy
displ  1.0000000 -0.897358 -0.779753
cty    -0.897358  1.000000  0.918382
hwy    -0.779753  0.918382  1.000000
```

**Explanation of Output:**
- `displ` and `cty` have a strong negative correlation of `-0.90`, meaning that as the engine displacement increases, the city mileage tends to decrease.
- `cty` and `hwy` have a strong positive correlation of `0.92`, meaning that cars with higher city mileage tend to also have higher highway mileage.
- `displ` and `hwy` have a moderate negative correlation of `-0.78`, showing that larger engines tend to have lower highway mileage.

---

### Step 4: Create Visualizations

#### Scatter Plot: Engine Displacement vs Highway Mileage

A scatter plot is a great way to visualize the relationship between two continuous variables. Let's plot `displ` (engine displacement) against `hwy` (highway mileage).

```r
# Scatter plot of Displacement vs Highway Mileage
ggplot(mpg, aes(x = displ, y = hwy)) +
  # Color points by car class
  geom_point(aes(color = class)) +  
  labs(
    title = "Engine Displacement vs Highway Mileage",
    x = "Engine Displacement (L)",
    y = "Highway Mileage (mpg)") +
    theme_minimal()
```

**Explanation:**
- `ggplot(mpg, aes(x = displ, y = hwy))` sets up the plot with `displ` on the x-axis and `hwy` on the y-axis.
- `geom_point(aes(color = class))` creates a scatter plot where each point is colored by the `class` variable (the car's class).
- `labs()` adds a title and axis labels.
- `theme_minimal()` applies a minimal theme for a cleaner plot.

**Output:**
This plot will display a scatter plot of engine displacement versus highway mileage, with points colored by car class.

---

#### Boxplot: Highway Mileage by Cylinder Count

A boxplot is useful for comparing the distribution of a numeric variable across different categories. We can use it to compare `hwy` (highway mileage) across different values of `cyl` (number of cylinders).

```r
# Boxplot of Highway Mileage by Number of Cylinders
ggplot(mpg, aes(x = factor(cyl), 
   y = hwy, fill = factor(cyl))) +
    geom_boxplot() +
    labs(title = 
        "Highway Mileage by Number of Cylinders", 
    x = "Number of Cylinders", 
    y = "Highway Mileage (mpg)") +
  theme_minimal()
```

**Explanation:**
- `aes(x = factor(cyl), y = hwy, fill = factor(cyl))` specifies that we want to plot `hwy` (highway mileage) for each category of `cyl` (number of cylinders). The `factor(cyl)` ensures that `cyl` is treated as a categorical variable.
- `geom_boxplot()` creates the boxplot, which shows the distribution of `hwy` for each cylinder category.
- `labs()` adds the title and axis labels.
- `theme_minimal()` ensures a clean, simple plot design.

**Output:**
The boxplot will display how highway mileage varies across different cylinder categories, showing the median, interquartile range, and potential outliers.

---

#### Histogram: Distribution of City Mileage

We can create a histogram to visualize the distribution of `cty` (city mileage).

```r
# Histogram of City Mileage
ggplot(mpg, aes(x = cty)) +
  geom_histogram(binwidth = 1,
    fill = "skyblue", 
    color = "black") +
    labs(title = 
        "Distribution of City Mileage",
    x = "City Mileage (mpg)",
    y = "Frequency") +
    theme_minimal()
```

**Explanation:**
- `aes(x = cty)` specifies that we want to plot the distribution of `cty` (city mileage).
- `geom_histogram()` creates the histogram. `binwidth = 1` controls the width of the bins, and `fill = "skyblue"` sets the color of the bars.
- `labs()` adds a title and axis labels.
- `theme_minimal()` applies a minimalistic theme for the plot.

**Output:**
This histogram will display how city mileage is distributed in the dataset, showing the frequency of different mileage values.

---

### Step 5: Conclusion

In this lesson, we have:
- Loaded the `mpg` dataset from the `ggplot2` package.
- Calculated correlations between key numeric variables like `displ`, `cty`, and `hwy`.
- Created scatter plots, boxplots, and histograms to explore the relationships and distributions of the data.

These techniques are

 fundamental for performing exploratory data analysis (EDA) in R, which is crucial for understanding your data before moving on to more complex analyses.

---

### Next Steps:
- You can explore other visualizations such as density plots or bar charts.
- You may want to try building models to predict variables like `mpg` using linear regression or machine learning techniques.