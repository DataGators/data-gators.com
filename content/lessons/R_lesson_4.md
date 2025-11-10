+++
title = "R_Lesson_4"
subtitle = "Working with Iris Dataset: Advanced Visualizations: Decision Trees, Correlation Plots and more"
draft = false
+++

## 📘 Overview

Data Visualization and Analysis with the Iris Dataset
In this lesson, we will explore the **Iris dataset** using R. We will create advanced visualizations, including **decision trees** and **correlation plots**, to analyze the relationships between different features of the dataset.

[Link to the Colab Template](https://colab.research.google.com/drive/11L2UmND5XxrT3kAVJHZjZxQl5KPTOpGw?usp=sharing)

**Expected Duration:** 45 minutes

This is an introductory lesson - no prior R knowledge required!
Required packages:
- datasets
- ggplot2

## 📦 Package Installation

In this lesson, we will be using the `datasets`, `ggplot2`, `corrplot`, and `car` packages for our code. Let's start!

```r
# Clear environment, plots, and console
rm(list = ls())
graphics.off()
cat("\014")

# Load necessary libraries
packages <- c("tidyverse", "ggplot2", "car", "caret", "rpart", "rpart.plot")

for (pkg in packages) {
  if (!require(pkg, character.only = TRUE)) {
    install.packages(pkg, dependencies = TRUE)
    library(pkg, character.only = TRUE)
  }
}
```

**Explanation:**

- `if (!require(pkg))` checks to see if all of the packages are installed.
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

### Step 4: Decison Tree Visualization

In this step, we will create a decision tree to visualize the relationships between the features of the iris dataset. We will use the `rpart` package to build the decision tree and `rpart.plot` to visualize it. The decision tree will help us understand how the different features contribute to the classification of the iris species. Training the decision tree model is important for understanding the relationships between the features and the target variable (Species).

```r
# Split data into training (80%) and testing (20%)
train_index <- createDataPartition(iris$Species, p = 0.8, list = FALSE)
train_data <- iris[train_index, ]
test_data <- iris[-train_index, ]

# Train decision tree model
tree_model <- rpart(Species ~ .,
                    data = train_data,
                    method = "class",
                    control = rpart.control(minsplit = 10, cp = 0.01))
# Plot the decision tree
rpart.plot(tree_model, box.palette = "auto", nn = TRUE)
```

**Explanation:**
-  Trains a decision tree to predict Species using all available features.
- `minsplit = 10`: Ensures that each node has at least 10 samples before it can split.
- `cp = 0.01`: Controls tree complexity, preventing overfitting.
- `rpart.plot()` displays the decision tree, making it easy to interpret.


### Step 5: Evaluate the Decision Tree Model

Now that we have trained the decision tree model, we will evaluate its performance using the test data. We will make predictions on the test data and compute a confusion matrix to assess the accuracy of the model. This step is crucial for understanding how well the model generalizes to unseen data.

```r
# Make predictions
predictions <- predict(tree_model, test_data, type = "class")

# Compute confusion matrix
conf_matrix <- confusionMatrix(predictions, test_data$Species)
print(conf_matrix)
```

**Explanation:**
- Once the decision tree model has been trained, we need to evaluate its performance on unseen test data. This step ensures that the model generalizes well and isn't just memorizing the training data.

- `predict(tree_model, test_data, type = "class")`

    - Uses the trained decision tree (tree_model) to make predictions on the test set.
    - The type = "class" argument ensures that the output is a categorical class label (e.g., "setosa", "versicolor", "virginica") rather than probabilities.

- `confusionMatrix(predictions, test_data$Species)`

    - Compares the model’s predictions to the actual species labels in the test data.
    - Outputs a confusion matrix, which provides key performance metrics:


### Step 6: Pruning the Decision Tree

Pruning is an essential step in decision tree modeling to prevent overfitting. In this step, we will prune the decision tree using the complexity parameter (cp) to find the optimal tree size. We will visualize the pruned tree and evaluate its performance. This can help us achieve a balance between model complexity and accuracy.

```r
# Print complexity parameter table
printcp(tree_model)

# Identify optimal cp value (minimizing xerror)
optimal_cp <- tree_model$cptable[which.min(tree_model$cptable[, "xerror"]), "CP"]

# Prune the tree using optimal cp
pruned_tree <- prune(tree_model, cp = optimal_cp)

# Plot pruned tree
rpart.plot(pruned_tree)
```

**Explanation:**
- `printcp(tree_model)`: Displays tree complexity values.
- Identifies the optimal complexity parameter (cp) where xerror is minimized.
- `prune()` removes unnecessary branches, making the model simpler and more generalizable.


### Step 7: Tuning the Decision Tree Model

In this step, we will perform hyperparameter tuning on the decision tree model using cross-validation. We will use the `caret` package to find the best complexity parameter (cp) for the decision tree. This step is crucial for optimizing the model's performance and ensuring it generalizes well to unseen data.

```r
# Perform 10-fold cross-validation to tune cp
control <- trainControl(method = "cv", number = 10)

tuned_tree <- train(Species ~ .,
                    data = iris,
                    method = "rpart",
                    trControl = control,
                    tuneGrid = expand.grid(cp = seq(0.01, 0.1, 0.01)))

# Print tuned model
print(tuned_tree)
```

**Explanation**
In this step, we use cross-validation to find the best complexity parameter (cp) for our decision tree. This ensures the model generalizes well to new data rather than overfitting to the training set.

What is Cross-Validation?
- Cross-validation is a technique used to assess a model’s performance by splitting the dataset into multiple training and validation subsets.
- Instead of using a single test set, it trains and evaluates the model on different portions of the data multiple times, leading to a more reliable evaluation.

What Does This Code Do?

1. `trainControl(method = "cv", number = 10)`

- Specifies 10-fold cross-validation, meaning the dataset is split into 10 parts (or folds).
- The model is trained on 9 folds and tested on the remaining fold. This process repeats 10 times, and the results are averaged for robustness.

2. `train(Species ~ ., data = iris, method = "rpart", trControl = control, tuneGrid = expand.grid(cp = seq(0.01, 0.1, 0.01)))`

- Uses the `train()` function from caret to automatically find the best cp value.
- The `tuneGrid` argument tests different `cp` values between 0.01 and 0.1 in increments of 0.01.
- The model is trained and evaluated at each cp value, and the best one is selected based on performance metrics.

3. `print(tuned_tree)`

- Displays the best cp value found during cross-validation.
- Helps determine the most optimal tree complexity for better performance.

Why is This Important?

- Prevents overfitting (tree too complex) and underfitting (tree too simple).
- Provides a data-driven approach to select the best hyperparameter (cp) instead of guessing.
- Ensures the decision tree is generalized for real-world data, not just training samples.

### Step 8: Correlation Plot

Correlations are important in understanding the relationships between different features in the dataset. In this step, we will compute the correlation matrix for the numerical features of the iris dataset and visualize it using a correlation plot. This will help us identify any strong correlations between features, which can be useful for feature selection and understanding the data better.

```r
# Compute correlation matrix for numerical features
cor_matrix <- cor(iris[,1:4])
print(cor_matrix)

# Visualize correlation matrix
corrplot::corrplot(cor_matrix, method = "circle", type = "upper")
```

**Explanation**
- `cor()` computes correlations between numerical variables (sepal/petal dimensions).
- `corrplot()` visualizes correlations in a heatmap.
- Helps identify which features are strongly related.
- Useful for decision tree modeling and feature selection.

### Step 9: Scatterplots for Virginica Species

In this step, we will create scatterplots to visualize the relationships between different features of the iris dataset, specifically for the Virginica species. Scatterplots are useful for understanding how two continuous variables relate to each other and can help identify patterns or trends in the data.

```r
# Filter dataset for Virginica species
iris_virginica <- filter(iris, Species == "virginica")

# Scatterplot: Sepal Width vs. Sepal Length
ggplot(iris_virginica, aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point() +
  ggtitle("Sepal Width vs. Sepal Length - Virginica")

# Scatterplot: Petal Width vs. Petal Length
ggplot(iris_virginica, aes(x = Petal.Width, y = Petal.Length)) +
  geom_point() +
  ggtitle("Petal Width vs. Petal Length - Virginica")
```

**Explanation**

- Filters dataset to include only Virginica species.

- 'ggplot()' creates scatterplots.

- Helps visualize relationships between petal/sepal dimensions.