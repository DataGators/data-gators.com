+++
title = 'Intro_iris'
date = 2024-11-24T03:03:29-05:00
draft = false
+++

## Introduction to R Programming with the Iris Dataset

Let's work with the built-in `iris` dataset, which contains data on 150 different species of iris flowers. Each entry contains the following measurements for the flowers:

- Sepal length
- Sepal width
- Petal length
- Petal width
- Species

We will go through the steps of loading the dataset, calculating basic correlations, and creating visualizations to explore the relationships between the different features.

---

### Step 1: Load the Iris Dataset

In R, you don't need to load the `iris` dataset manually because it is built-in. We can directly access it.

```r
# Load the iris dataset
data(iris)

# Check the first few rows of the dataset
head(iris)
```

**Explanation:**
- `data(iris)` loads the dataset into memory.
- `head(iris)` displays the first six rows of the `iris` dataset to give us an idea of its structure.

**Output:**
```
  Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
1           5.1          3.5           1.4          0.2     setosa
2           4.9          3.0           1.4          0.2     setosa
3           4.7          3.2           1.3          0.2     setosa
4           4.6          3.1           1.5          0.2     setosa
5           5.0          3.6           1.4          0.2     setosa
6           5.4          3.9           1.7          0.4     setosa
```


### Step 2: Calculate Correlations

We can calculate the correlation between numeric variables in the dataset to explore how they are related.

```r
# Calculate correlations between 
# numeric features (excluding the Species column)
cor(iris[, 1:4])
```

**Explanation:**
- `iris[, 1:4]` selects all rows and the first four columns (which contain the numeric data: sepal length, sepal width, petal length, and petal width).
- `cor()` computes the correlation matrix, which measures the linear relationships between pairs of variables.

**Output:**
```
               Sepal.Length Sepal.Width Petal.Length Petal.Width
Sepal.Length        1.000000   -0.117570     0.871754     0.817941
Sepal.Width        -0.117570    1.000000    -0.428440    -0.366125
Petal.Length        0.871754   -0.428440     1.000000     0.962865
Petal.Width         0.817941   -0.366125     0.962865     1.000000
```

**Explanation of Output:**

- The correlation values range from -1 to 1, where:
  
  - `1` indicates a perfect positive correlation.
  - `-1` indicates a perfect negative correlation.
  - `0` indicates no linear correlation.

- For example, Sepal Length and Petal Length have a high positive correlation (`0.87`), while Sepal Width and Petal Length have a moderate negative correlation (`-0.43`).

---

### Step 3: Create Basic Plots

#### Scatter Plot: Sepal Length vs Petal Length

We can plot Sepal Length against Petal Length to visualize their relationship.

```r
# Scatter plot of Sepal Length vs Petal Length
plot(iris$Sepal.Length, iris$Petal.Length, 
     main = "
        Scatter Plot of Sepal Length vs Petal Length",
     xlab = "Sepal Length",
     ylab = "Petal Length", 
     pch = 19,
     col = iris$Species)
```

**Explanation:**

- `plot()` is used to create a scatter plot. The `pch = 19` argument sets the point type (solid circle), and `col = iris$Species` colors the points according to the flower species.
- `main`, `xlab`, and `ylab` set the title and axis labels for the plot.

**Output:**
This plot will show a scatter plot where each point represents a flower, and the color indicates the species.

---

#### Boxplot: Sepal Length by Species

A boxplot allows us to compare the distribution of sepal length across different species.

```r
# Boxplot of Sepal Length grouped by Species
boxplot(Sepal.Length ~ Species, data = iris,
        main = 
          "Boxplot of Sepal Length by Species",
        xlab = "Species",
        ylab = "Sepal Length",
        col = c(
          "lightblue", 
          "lightgreen",
          "lightcoral")
          )
```

**Explanation:**
- `Sepal.Length ~ Species` specifies that we want to plot Sepal Length for each species.
- `boxplot()` creates the boxplot, and the `col` argument specifies the colors for each species group.

**Output:**
This boxplot will display three boxes, one for each species, showing the range, median, and interquartile range of Sepal Length.

---

#### Pair Plot: Visualizing Relationships Between All Variables

To explore the relationships between all the numeric variables simultaneously, we can use a pair plot (also known as a scatterplot matrix).

```r
# Install and load the GGally 
# package if not already installed
# install.packages("GGally")
library(GGally)

# Create a pair plot of the numeric variables
ggpairs(iris[, 1:4],
  aes(color = iris$Species))
```

**Explanation:**
- The `ggpairs()` function from the `GGally` package creates a matrix of scatter plots, showing the pairwise relationships between all numeric columns in the `iris` dataset.
- The `aes(color = iris$Species)` argument colors the points by species, making it easier to distinguish between the species in the plots.

**Output:**
This will generate a matrix of scatter plots, histograms, and correlations, making it easy to visually inspect the relationships between sepal length, sepal width, petal length, and petal width.

---

### Step 4: Conclusion

In this lesson, we have:
- Loaded and explored the `iris` dataset.
- Calculated correlations between numeric variables.
- Created scatter plots, boxplots, and pair plots to visualize relationships between the features.

This exercise helps in understanding both the basic analysis and visualization techniques in R, which are essential for data exploration in data science.

---

### Next Steps:
- You can try modifying the plots to explore other variables or try additional visualizations like histograms, density plots, or heatmaps.
- Experiment with different `ggplot2` functions for more advanced visualizations.
