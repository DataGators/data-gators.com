+++
title = "Python Data Analytics Lesson 1"
subtitle = "Loading and Understanding Your Data"
draft = false
+++

# Data Analytics Lesson 1: Loading and Understanding Your Data

## 📘 Overview

Welcome to your first data analytics lesson! Data analytics is like being a detective - we use data to answer questions and discover patterns. Today, we'll learn how to load data into Python and take our first look at what we're working with.

**Expected Duration:** 30-45 minutes  
This is an introductory lesson - no prior Python knowledge required!

Required packages:
- pandas
- numpy

## 📦 Package Installation

Before we start, we need to install some special Python tools (called libraries) that help us work with data:

```python
# These are the tools we'll use for data analysis
import pandas as pd    # For working with data tables
import numpy as np     # For math operations
import os             # For working with files

# Don't worry if you see warnings - that's normal!
print("Libraries imported successfully!")
```

**Explanation:**

- `pandas` is a powerful library for working with tabular data (like spreadsheets)
- `numpy` provides mathematical operations and array handling
- `os` helps us work with files and directories

## 📌 Loading Your First Dataset

Let's start with a simple dataset about students and their test scores. In real life, data often comes in CSV files (Comma Separated Values).

```python
# First, let's create some sample student data to practice with
student_data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eva', 'Frank', 'Grace', 'Henry'],
    'Age': [16, 17, 16, 18, 17, 16, 17, 18],
    'Grade': ['10th', '11th', '10th', '12th', '11th', '10th', '11th', '12th'],
    'Math_Score': [85, 92, 78, 95, 88, 76, 91, 87],
    'Science_Score': [89, 87, 82, 98, 85, 79, 93, 89],
    'English_Score': [92, 85, 88, 91, 94, 83, 89, 86]
}

# Convert this into a DataFrame (think of it as a digital spreadsheet)
df = pd.DataFrame(student_data)

# Let's see what our data looks like
print("Our Student Dataset:")
print(df)
```

**What's happening here?**

- We create a Python dictionary with student information
- `pd.DataFrame()` converts our dictionary into a pandas DataFrame (like a spreadsheet)
- The `print()` function displays our data

## 🔍 Understanding Your Data Structure

Now let's explore what we have. Think of this like getting to know a new friend - we want to learn basic facts about our data:

```python
# How big is our dataset? (rows and columns)
print("Dataset shape (rows, columns):", df.shape)

# What types of information do we have?
print("\nColumn names:")
print(df.columns.tolist())

# What kind of data is in each column?
print("\nData types:")
print(df.dtypes)
```

**Understanding the output:**

- `.shape` tells us the dimensions: (rows, columns)
- `.columns` lists all column names
- `.dtypes` shows the data type of each column (numbers, text, etc.)

## 📊 Getting Basic Information About Your Data

Let's learn some basic facts about our student data:

### Statistical Summary

```python
# Get a quick summary of numerical columns
print("Basic Statistics Summary:")
print(df.describe())
```

The `.describe()` method gives us:
- **count:** Number of non-empty values
- **mean:** Average value
- **std:** Standard deviation (how spread out the values are)
- **min/max:** Smallest and largest values
- **25%, 50%, 75%:** Quartile values

### Counting Values

```python
# Count how many students are in each grade
print("Students per grade:")
print(df['Grade'].value_counts())
```

### Viewing Portions of Data

```python
# Look at just the first few rows (useful for large datasets)
print("First 3 students:")
print(df.head(3))

# Look at the last few rows
print("\nLast 3 students:")
print(df.tail(3))
```

**Why is this useful?**

- `.head(n)` shows the first n rows - great for previewing large datasets
- `.tail(n)` shows the last n rows - useful for checking if data loaded completely

## 💡 Practice Exercise

Now it's your turn! Try these exercises to practice what you've learned:

### Exercise 1: Create Your Own Dataset

```python
# Create your own small dataset about your friends or family
# Include at least 3 columns and 5 rows
# Replace this with your own data!

my_data = {
    'Name': ['Person1', 'Person2', 'Person3', 'Person4', 'Person5'],
    'Age': [20, 25, 30, 22, 28],
    'Hobby': ['Reading', 'Sports', 'Music', 'Art', 'Cooking']
}

my_df = pd.DataFrame(my_data)
print("My Dataset:")
print(my_df)
```

### Exercise 2: Explore Your Dataset

```python
# Use the methods we learned to understand your data

print("Shape of my dataset:", my_df.shape)
print("\nData types:")
print(my_df.dtypes)
print("\nFirst 2 rows:")
print(my_df.head(2))
```

## ✅ Summary

Congratulations! You've completed your first data analytics lesson. Here's what you learned:

1. **How to import essential libraries** (pandas, numpy)
2. **How to create a DataFrame** from dictionary data
3. **How to explore data structure** (shape, columns, data types)
4. **How to get basic statistics** using `.describe()`
5. **How to view portions of your data** using `.head()` and `.tail()`
6. **How to count values** using `.value_counts()`

## 🚀 Next Steps

In Lesson 2, we'll learn how to:
- Clean messy data
- Handle missing values
- Filter and sort our data
- Load data from real CSV files

Keep practicing with different datasets - the more you work with data, the more comfortable you'll become!

## 📓 Interactive Notebook

Want to practice with the interactive Jupyter notebook version of this lesson?

[Download the Jupyter Notebook](/files/my_analysis_project/data-analytics-01-loading-data.ipynb)

**Tip:** You can run this notebook in:
- Google Colab (free, no installation needed)
- Jupyter Lab on your computer
- VS Code with Jupyter extension
