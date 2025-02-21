
+++

title = 'Data Pre-processing with Iris Dataset'

date = 2024-11-24T03:03:29-05:00

draft = false

+++

## Introduction to Pre-Processing data in R with the Iris Dataset

Let's work with the built-in `iris` dataset, which contains data on 150 different species of iris flowers. Each entry contains the following measurements for the flowers:

- Sepal length
- Sepal width
- Petal length
- Petal width
- Species

We will break down the basic steps that make up loading, viewing, and cleaning up the dataset. At the end, we will see the benefit and importance of having a clean dataset, and how failing to adequately do so can cause issues with our analyses.

---

### Step 1: Load the required packages

In this lesson, we will be using the `datasets` and `ggplot2` packages for our code. Let's start!

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

```

**Explanation:**

- `if (!require(datasets))` checks to see if the datasets package is installed. The same goes for the same statement checking to see if `ggplot2` is installed.
- `install.packages()` is used to install the corresponding package that is included within the parentheses.
- `library()` is used to load the methods or functions within the specified package for use within code. Without this command, we would not be able to use the methods contained in the package.

### Step 2: Load and Check Data

We are going to load in the dataset, and then check its structure and dimensions.

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

**Output:**

```r

First few rows of the iris dataset:

  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa

Structure of the iris dataset:

'data.frame':   150 obs. of  5 variables:
 $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
 $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
 $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
 $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
 $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

```

### Step 3: Checking the data

Now that we've got the dataset loaded, and can draw some basic information from it, let's make sure there's no missing or irregular values.

```r

cat("\nChecking for NA values in the dataset:\n")
print(any(is.na(iris)))

cat("\nChecking for duplicate rows in the dataset:\n")
print(any(duplicated(iris)))

cat("\nBoxplot for Sepal.Length to check for outliers:\n")
boxplot(iris$Sepal.Length, main="Boxplot for Sepal Length", ylab="Sepal Length")

cat("\nLevels of the Species factor:\n")
print(levels(iris$Species))

cat("\nChecking for zero or negative values in Sepal.Length:\n")
print(any(iris$Sepal.Length <= 0))

```

**Explanation:**

-`plot()` is used to create a scatter plot. The `pch = 19` argument sets the point type (solid circle), and `col = iris$Species` colors the points according to the flower species.

-`main`, `xlab`, and `ylab` set the title and axis labels for the plot.

**Output:**

This plot will show a scatter plot where each point represents a flower, and the color indicates the species.

---

#### Boxplot: Sepal Length by Species

A boxplot allows us to compare the distribution of sepal length across different species.

```r

# Boxplot of Sepal Length grouped by Species

boxplot(Sepal.Length ~ Species, data= iris,

        main=

          "Boxplot of Sepal Length by Species",

        xlab="Species",

        ylab="Sepal Length",

        col=c(

          "lightblue", 

          "lightgreen",

          "lightcoral")

          )

```

**Explanation:**

-`Sepal.Length ~ Species` specifies that we want to plot Sepal Length for each species.

-`boxplot()` creates the boxplot, and the `col` argument specifies the colors for each species group.

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

  aes(color= iris$Species))

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
