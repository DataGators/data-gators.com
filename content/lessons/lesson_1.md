+++
title = "Lesson 1"
subtitle = "Data Pre-processing with Iris Dataset"
draft = false
+++

# Pre-Processing data in R with the Iris Dataset

## 📘 Overview

Data pre-processing is an important part of any aspect of data analysis. Regardless of what field of study you may find yourself in, ensuring the validity of your data is paramount to establishing a credible basis to your work. Otherwise, it can lead to inaccurate results and provide you with the wrong information for your project or study.

{{% notice info %}}
**Expected Duration:** 45 minutes

This is an introductory lesson - no prior R knowledge required!
Required packages:
- datasets
- ggplot2
{{% /notice %}}

## 📦 Package Installation

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

- `if (!require(datasets))` checks to see if the `datasets` package is installed. The same goes for the same statement checking to see if `ggplot2` is installed.
- `install.packages()` is used to install the corresponding package that is included within the parentheses.
- `library()` is used to load the methods or functions within the specified package for use in our code. Without this command, we would not be able to use the methods contained in the package.

## 📌 Dataset Overview

For this lesson, let's work with the built-in `iris` dataset, which contains data on 50 iris flowers across 3 different species of iris flowers. The `iris` dataset is a product of a botanical study carried out in 1936 by Ronald Fisher called *The use of multiple measurements in taxonimic problems*. The data of this study we will be using contains variables on respective length and width of petals and sepals of iris flower.

Each entry contains the following measurements (as **averages**, except *species*) for the flowers:

- **Sepal length** - The length of the sepal leaves (in cm)
- **Sepal width** - The width of the sepal leaves (in cm)
- **Petal length** - The length of the petal (in cm)
- **Petal width** - The width of the petals (in cm)
- **Species** - The respective species of the flower

We will break down the basic steps that make up pre-processing the dataset. At the end, we will see the benefit and importance of having a adequately prepared dataset, and how failing to adequately do so can cause issues with our analyses.

## 🛠 Data Pre-Processing Steps

### Step 1: Load and Preview Data

We are going to load in the dataset, and then look over its structure and dimensions.

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

***NOTE:*** If you look at the output, you will see that there is a datatype associated with each of the variables as the first textual tag of each variable of the output of `str(iris)`. Take note that the datatype of the *Species* variable is `Factor`. This datatype is unique to R, and is specifically for categorical variables, in this case being the species of iris flower.

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

- `sum()` will calculate the sum of the inserted condition within the bounds of the specified object. In the example, we prompted for a sum of all applicable `duplicated()` objects within the dataset, and we were given a count of a signular duplicated entry within the dataset.

- `boxplot()` creates a boxplot of the `Sepal.Length` variable within the dataset given the argument of `iris$Sepal.Length`.

- `levels()` returns the levels of the `Species` variable within the dataset given the argument of `iris$Species`.

- `any(iris$Sepal.Length <= 0)` checks to see if any of the values within the `Sepal.Length` variable are zero or negative.

**Output:**

```r

Checking for NA values in the dataset:
[1] FALSE

Checking for duplicate rows in the dataset:
[1] TRUE
[1] 1
Boxplot for Sepal.Length to check for outliers:
[Bloxplot image is attached below]

Levels of the Species factor:
[1] "setosa"     "versicolor" "virginica"

Checking for zero or negative values in Sepal.Length:
[1] FALSE

```

![image](img/image.png)

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

**Output:**

```r

Removing duplicate rows from the dataset:

[1] FALSE

'data.frame':   149 obs. of  5 variables:
 $ Sepal.Length: num  5.1 4.9 4.7 4.6 5 5.4 4.6 5 4.4 4.9 ...
 $ Sepal.Width : num  3.5 3 3.2 3.1 3.6 3.9 3.4 3.4 2.9 3.1 ...
 $ Petal.Length: num  1.4 1.4 1.3 1.5 1.4 1.7 1.4 1.5 1.4 1.5 ...
 $ Petal.Width : num  0.2 0.2 0.2 0.2 0.2 0.4 0.3 0.2 0.2 0.1 ...
 $ Species     : Factor w/ 3 levels "setosa","versicolor",..: 1 1 1 1 1 1 1 1 1 1 ...

```

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
cat("Number of species setosa dataset:", length(unique(iris_setosa$Species)), "\n")
cat("Number of species versicolor dataset:", length(unique(iris_versicolor$Species)), "\n")
cat("Number of species in the virginica dataset:", length(unique(iris_virginica$Species)), "\n")

```

**Explanation:**

- `iris[iris$Species == "setosa", ]` creates a new dataset that contains all the data associated with the *setosa* species in the dataset. The same action is done for the *versicolor* and *virginica* datasets, respectively.
- `length(unique(iris_setosa$Species))` Will give us the amount of species within the `iris_setosa` dataset. We are doing this to make sure that there is only one species in the dataset, and that we properly executed the code.

**Output:**

```r

 Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa
   Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
51          7.0         3.2          4.7         1.4 versicolor
52          6.4         3.2          4.5         1.5 versicolor
53          6.9         3.1          4.9         1.5 versicolor
54          5.5         2.3          4.0         1.3 versicolor
55          6.5         2.8          4.6         1.5 versicolor
56          5.7         2.8          4.5         1.3 versicolor
    Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
101          6.3         3.3          6.0         2.5 virginica
102          5.8         2.7          5.1         1.9 virginica
103          7.1         3.0          5.9         2.1 virginica
104          6.3         2.9          5.6         1.8 virginica
105          6.5         3.0          5.8         2.2 virginica
106          7.6         3.0          6.6         2.1 virginica
Number of species setosa dataset: 1 
Number of species versicolor dataset: 1 
Number of species in the virginica dataset: 1 

```

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

**Output:**

![image](https://github.com/user-attachments/assets/965f0053-6c1d-4596-9855-b289435290ba)
![image](https://github.com/user-attachments/assets/0f8dfe99-59a9-4f30-b398-d9f3991f52a4)
![image](https://github.com/user-attachments/assets/27672656-2f1f-4a2d-b73a-7b373ff8e649)
![image](https://github.com/user-attachments/assets/0d59a1f9-bbb7-40e9-bd7d-64c788eb12a6)
![image](https://github.com/user-attachments/assets/707173ef-30c0-4010-bf58-eb173800e23c)
![image](https://github.com/user-attachments/assets/54e7abb8-04c0-41b2-b5f4-b6ab525e9c93)

## Part 2: Exploration with Comprehensive Data
In this next part of the lesson, we will be looking at a new dataset, this time with a more robust and comprehensive set of variables and information.

<p><a href="https://data.cityofnewyork.us/Environment/Air-Quality/c3uy-2p5r/about_data" target="_blank">Click here to check out the datset we'll be working with next.</a></p></div>

![image](https://github.com/user-attachments/assets/13a79b17-deca-492e-a1df-be5c8aada224)
If you get set to a redirect page like the one above, click the first link to continue to the landing page for the dataset we are working with. This is simply a redirect page, you haven't made a mistake.

Now that you've taken a brief look at the data, let's take the first steps to working with it.
```r
nyc_airq <- read.csv(url("https://data.cityofnewyork.us/resource/c3uy-2p5r.csv"))
```

**Explanation**
- `nyc_airq <-` creates a new object with the name *nyc_airq*. `<-` act as an assignor, giving the *nyc_airq* object a value of the dataset we are reading in from a url. If you have worked with other languages in the past, it is similar to the `=` assigment operator. However, both Exist in R, and they have different meanings.
    - When using `=` inside a function call, the assignment is local to the function and does not affect the global environment. For example, `mean(x = 1:10)` calculates the mean of the vector 1:10 but does not create a variable x in the global environment.
    - When using `<-` inside a function call, the assignment is made to the global environment. For example, `mean(x <- 1:10)` creates a variable x in the global environment with the value `1:10`.
    - The `<-` operator has a higher precedence than `=`. This means that in expressions where both operators are used, `<-` will be evaluated first. For example, `x = y <- 5` is interpreted as `x = (y <- 5`), which assigns 5 to y and then assigns y to x.
- `read.csv(url("https://data.cityofnewyork.us/resource/c3uy-2p5r.csv")` creates a csv from a url object, in this case it is our dataset for our next activity. If you happen to use this function in the future, you have to make sure that the url leads to a *CSV* (Comma Separated Variable) file type format.

Now that we've loaded up the dataset, let's take a look at it in the same way we did with the `iris` dataset.

```r

# Display the first few rows of the dataset
cat("\nFirst few rows of the NYC Air Quality dataset:\n")
print(head(nyc_airq))

# Display the structure of the dataset
cat("\nStructure of the NYC Air Quality dataset:\n")
str(nyr_airq)


```


#### Results of cleaning the data

As you can see, in comparison to the `str(iris)` in step 2, once we've removed the duplicate row, there now are 149 observations instead of 150, and there are now no outliers or other abnormal values. Now the data is ready for analysis!

### Step 5: Conclusion

In this lesson, we've gone through the basics of loading and exploring the `iris` dataset in R. We've also seen how to check for missing values, outliers, and duplicate rows in the dataset.
We've also seen how to remove duplicate rows from the dataset, and how to use `ggplot2` to create visualizations of the data. It is an important practice to check your data once you've found a source that you're comfortable with, and that you feel is reliable and accurate. Even small abnormalities within data can cause issues with your analyses, and might be hidden until you visualize the data and come across them. Being sure to check your data through visualizations and pre-processing is an important part of ensuring that your analyses are accurate and reliable.

Good work, you've learned the basics of data pre-processing in R!



### What's Next?

- You can try modifying the plots to explore other variables or try additional visualizations like histograms, density plots, or heatmaps.
- Experiment with different `ggplot2` functions for more advanced visualizations.
- Try out graphing specific attributes of the dataset, or create your own visualizations to illustrate interactions between variables.
