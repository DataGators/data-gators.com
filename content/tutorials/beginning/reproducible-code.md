---
title: "Writing Clean, Reproducible Research Code"
date: 2026-01-26
draft: false
description: "Learn best practices for organizing code, writing functions, documenting your work, and making your research reproducible"
tags: ["coding", "best-practices", "reproducibility", "documentation", "python", "R"]
categories: ["Getting Started", "Best Practices"]
weight: 2
---

## Why Clean Code Matters in Research

Here's a scenario: You finish an analysis, write a paper, submit it. Six months later, a reviewer asks you to re-run your analysis with slightly different parameters. You open your old code and... you have no idea what it does. Sound familiar?

Writing clean, reproducible code isn't just about being neat – it's about making sure your research can be verified, extended, and built upon. Let's learn how!

---

## The Golden Rules of Research Code

Before we dive into specifics, here are the principles that guide everything:

1. **Code is read more often than it's written** - Make it easy to understand
2. **Future you is a different person** - Document for them
3. **If it's not reproducible, it's not science** - Make sure others can run your code
4. **Simple is better than clever** - Clear code beats fancy code
5. **Organize early** - It's easier to stay organized than to clean up later

---

## Project Structure: Start Right

A good project structure makes everything easier. Here's a template that works:

```
my-research-project/
├── data/
│   ├── raw/              # Original, untouched data
│   ├── processed/        # Cleaned data ready for analysis
│   └── README.md         # Describe your data sources
├── notebooks/            # Jupyter/R Markdown notebooks
│   ├── 01-exploration.ipynb
│   ├── 02-cleaning.ipynb
│   └── 03-analysis.ipynb
├── src/                  # Reusable code/functions
│   ├── __init__.py
│   ├── data_loading.py
│   ├── preprocessing.py
│   └── visualization.py
├── outputs/              # Figures, tables, results
│   ├── figures/
│   └── tables/
├── tests/                # Test your functions!
│   └── test_preprocessing.py
├── requirements.txt      # Python packages
├── environment.yml       # Conda environment (alternative)
├── README.md            # Project overview
└── .gitignore           # Files to ignore in version control
```

### Create This Structure

**In Python:**

```python
import os

dirs = [
    'data/raw', 'data/processed',
    'notebooks', 'src', 'outputs/figures',
    'outputs/tables', 'tests'
]

for dir in dirs:
    os.makedirs(dir, exist_ok=True)
    
print("Project structure created! 🎉")
```

**In R:**

```r
dirs <- c(
  'data/raw', 'data/processed',
  'notebooks', 'R', 'outputs/figures',
  'outputs/tables', 'tests'
)

for (dir in dirs) {
  dir.create(dir, recursive = TRUE, showWarnings = FALSE)
}

cat("Project structure created! 🎉\n")
```

---

## Naming Things: It Matters More Than You Think

### Good vs Bad Names

**Bad:**

```python
# What do these mean?
d = load_data('file.csv')
x = d[d['v'] > 100]
result = x.mean()
```

**Good:**

```python
# Crystal clear!
customer_data = load_data('customer_purchases.csv')
high_value_customers = customer_data[customer_data['purchase_amount'] > 100]
average_high_value_purchase = high_value_customers['purchase_amount'].mean()
```

### Naming Conventions

**Python (PEP 8):**

```python
# Variables and functions: lowercase_with_underscores
customer_count = 100
def calculate_average_age(data):
    pass

# Classes: CapitalizedWords
class DataProcessor:
    pass

# Constants: UPPERCASE_WITH_UNDERSCORES
MAX_ITERATIONS = 1000
DEFAULT_BATCH_SIZE = 32
```

**R (tidyverse style):**

```r
# Variables and functions: lowercase_with_underscores
customer_count <- 100
calculate_average_age <- function(data) {
  # function body
}

# Avoid dots in names (old R style)
# Bad: customer.count
# Good: customer_count
```

---

## Writing Functions: Don't Repeat Yourself

If you're copying and pasting code, you need a function!

### Python Example

**Before (repetitive):**

```python
# Loading data three times with same steps
data1 = pd.read_csv('jan.csv')
data1 = data1.dropna()
data1['date'] = pd.to_datetime(data1['date'])

data2 = pd.read_csv('feb.csv')
data2 = data2.dropna()
data2['date'] = pd.to_datetime(data2['date'])

data3 = pd.read_csv('mar.csv')
data3 = data3.dropna()
data3['date'] = pd.to_datetime(data3['date'])
```

**After (using a function):**

```python
def load_and_clean_data(filepath):
    """
    Load a CSV file and perform standard cleaning.
    
    Parameters
    ----------
    filepath : str
        Path to the CSV file
        
    Returns
    -------
    pd.DataFrame
        Cleaned dataframe with parsed dates
    """
    data = pd.read_csv(filepath)
    data = data.dropna()
    data['date'] = pd.to_datetime(data['date'])
    return data

# Now use it!
data1 = load_and_clean_data('jan.csv')
data2 = load_and_clean_data('feb.csv')
data3 = load_and_clean_data('mar.csv')
```

### R Example

**Before:**

```r
# Repetitive cleaning
data1 <- read.csv('jan.csv') %>%
  na.omit() %>%
  mutate(date = as.Date(date))

data2 <- read.csv('feb.csv') %>%
  na.omit() %>%
  mutate(date = as.Date(date))
```

**After:**

```r
#' Load and clean data file
#'
#' @param filepath Path to CSV file
#' @return Cleaned tibble with parsed dates
load_and_clean_data <- function(filepath) {
  read.csv(filepath) %>%
    na.omit() %>%
    mutate(date = as.Date(date))
}

# Use it!
data1 <- load_and_clean_data('jan.csv')
data2 <- load_and_clean_data('feb.csv')
```

---

## Documentation: Your Future Self Will Thank You

### Docstrings for Functions

**Python (NumPy style):**

```python
def calculate_summary_statistics(data, column, groupby=None):
    """
    Calculate summary statistics for a column, optionally grouped.
    
    This function computes mean, median, std, min, and max for
    the specified column. If a groupby column is provided,
    statistics are calculated for each group.
    
    Parameters
    ----------
    data : pd.DataFrame
        Input dataframe
    column : str
        Name of column to analyze
    groupby : str, optional
        Column name to group by (default is None)
        
    Returns
    -------
    pd.DataFrame
        DataFrame with summary statistics
        
    Examples
    --------
    >>> df = pd.DataFrame({'age': [25, 30, 35], 'city': ['A', 'B', 'A']})
    >>> calculate_summary_statistics(df, 'age', groupby='city')
    """
    if groupby:
        return data.groupby(groupby)[column].describe()
    else:
        return data[column].describe()
```

**R (roxygen2 style):**

```r
#' Calculate Summary Statistics
#'
#' Compute summary statistics for a column, optionally grouped.
#'
#' @param data A data frame
#' @param column Name of column to analyze
#' @param groupby Optional column name to group by
#' @return A data frame with summary statistics
#' @examples
#' df <- data.frame(age = c(25, 30, 35), city = c('A', 'B', 'A'))
#' calculate_summary_statistics(df, 'age', groupby = 'city')
#' @export
calculate_summary_statistics <- function(data, column, groupby = NULL) {
  if (!is.null(groupby)) {
    data %>%
      group_by(across(all_of(groupby))) %>%
      summarise(across(all_of(column), 
                      list(mean = mean, median = median, 
                           sd = sd, min = min, max = max)))
  } else {
    data %>%
      summarise(across(all_of(column),
                      list(mean = mean, median = median,
                           sd = sd, min = min, max = max)))
  }
}
```

### Comment Wisely

**Bad comments (obvious):**

```python
# Increment i
i = i + 1

# Loop through data
for row in data:
    pass
```

**Good comments (explain why):**
```python
# Use robust scaling instead of standard scaling because
# our data contains many outliers that would skew results
from sklearn.preprocessing import RobustScaler
scaler = RobustScaler()

# Sample 10% because full dataset causes memory issues
# on machines with < 16GB RAM
sample_data = data.sample(frac=0.1, random_state=42)
```

---

## Making Your Analysis Reproducible

### Set Random Seeds

**Python:**

```python
import random
import numpy as np
import tensorflow as tf  # if using

# Set all random seeds
RANDOM_SEED = 42

random.seed(RANDOM_SEED)
np.random.seed(RANDOM_SEED)
tf.random.set_seed(RANDOM_SEED)  # if using TensorFlow
```

**R:**

```r
# Set random seed
set.seed(42)

# For parallel processing
library(doRNG)
registerDoRNG(42)
```

### Document Your Environment

**Python - requirements.txt:**

```bash
# Create requirements file
pip freeze > requirements.txt

# Others can install with:
# pip install -r requirements.txt
```

**Python - environment.yml (Conda):**

```bash
# Create environment file
conda env export > environment.yml

# Others can recreate with:
# conda env create -f environment.yml
```

**R - Using renv:**

```r
# Initialize renv for your project
renv::init()

# Save package versions
renv::snapshot()

# Others can restore with:
# renv::restore()
```

### Create a README

Every project needs a README.md:

```markdown
# Customer Churn Analysis

## Overview
This project analyzes customer churn patterns using machine learning.

## Setup

```bash
# Create environment
conda env create -f environment.yml
conda activate churn-analysis
```

## Data

Data is from [source]. Download from [URL] and place in `data/raw/`.

## Running the Analysis

1. Data cleaning: `python src/01_clean_data.py`
2. Exploration: Open `notebooks/02_explore.ipynb`
3. Modeling: `python src/03_train_model.py`

## Results

See `outputs/` for figures and tables.

## Citation

If you use this code, please cite: [your paper]

## Contact

Your Name - email@university.edu
```

---

## Version Control with Git

Git tracks changes and lets you undo mistakes. Essential!

### Basic Git Workflow

```bash
# Initialize repository
cd my-project
git init

# Create .gitignore first!
cat > .gitignore << EOF
# Python
__pycache__/
*.pyc
venv/
.ipynb_checkpoints/

# R
.Rhistory
.RData
.Rproj.user/

# Data (usually too large)
data/raw/*
data/processed/*

# Keep README files
!data/raw/README.md
!data/processed/README.md

# OS
.DS_Store
EOF

# Stage and commit
git add .
git commit -m "Initial commit: project structure"

# Continue working...
git add src/data_loading.py
git commit -m "Add data loading function"

# Push to GitHub (after creating repo on github.com)
git remote add origin https://github.com/yourusername/your-repo.git
git push -u origin main
```

### Good Commit Messages

**Bad:**

```text
- fixed stuff
- update
- asdfasdf
```

**Good:**

```text
- Add function to handle missing values in customer data
- Fix bug in date parsing that caused incorrect aggregations
- Update visualization colors for better accessibility
```

---

## Code Organization: Scripts vs Notebooks

### When to Use Notebooks

✅ **Good for:**

- Exploratory analysis
- Visualizations
- Explaining your process
- Teaching and presentations

❌ **Not good for:**

- Production code
- Functions you'll reuse
- Long, complex analyses
- Code you'll run repeatedly

### When to Use Scripts

✅ **Good for:**

- Reusable functions
- Data processing pipelines
- Functions you'll test
- Code run from command line

### The Best Approach: Both!

```python
# In src/data_processing.py (script)
def clean_customer_data(data):
    """Reusable cleaning function"""
    data = data.dropna(subset=['customer_id'])
    data['signup_date'] = pd.to_datetime(data['signup_date'])
    return data

# In notebooks/01_analysis.ipynb (notebook)
import sys
sys.path.append('../src')
from data_processing import clean_customer_data

# Now use your function in the notebook!
raw_data = pd.read_csv('../data/raw/customers.csv')
clean_data = clean_customer_data(raw_data)

# Continue with analysis and visualizations...
```

**Same in R:**

```r
# In R/data_processing.R (script)
clean_customer_data <- function(data) {
  data %>%
    filter(!is.na(customer_id)) %>%
    mutate(signup_date = as.Date(signup_date))
}

# In notebooks/01_analysis.Rmd (R Markdown)
source('../R/data_processing.R')

raw_data <- read.csv('../data/raw/customers.csv')
clean_data <- clean_customer_data(raw_data)
# Continue analysis...
```

---

## Testing Your Code

Yes, research code needs tests too!

### Simple Tests in Python

```python
# In src/data_processing.py
def calculate_age_from_birthdate(birthdate_str):
    """Calculate age from birthdate string (YYYY-MM-DD)"""
    from datetime import datetime
    birthdate = datetime.strptime(birthdate_str, '%Y-%m-%d')
    today = datetime.now()
    age = today.year - birthdate.year
    return age

# In tests/test_data_processing.py
import pytest
from src.data_processing import calculate_age_from_birthdate

def test_calculate_age():
    # Test with known birthdate
    age = calculate_age_from_birthdate('2000-01-01')
    assert age >= 24 and age <= 26  # Depends on current year
    
def test_calculate_age_invalid():
    # Test that invalid input raises error
    with pytest.raises(ValueError):
        calculate_age_from_birthdate('not-a-date')

# Run tests with: pytest tests/
```

### Simple Tests in R

```r
# In R/data_processing.R
calculate_age_from_birthdate <- function(birthdate_str) {
  birthdate <- as.Date(birthdate_str)
  today <- Sys.Date()
  age <- as.numeric(difftime(today, birthdate, units = "days")) / 365.25
  floor(age)
}

# In tests/testthat/test_data_processing.R
library(testthat)
source('../../R/data_processing.R')

test_that("age calculation works", {
  age <- calculate_age_from_birthdate('2000-01-01')
  expect_gte(age, 24)
  expect_lte(age, 26)
})

test_that("invalid date throws error", {
  expect_error(calculate_age_from_birthdate('not-a-date'))
})

# Run tests with: testthat::test_dir('tests')
```

---

## Common Pitfalls to Avoid

### 1. Hard-coded Paths

**Bad:**

```python
data = pd.read_csv('/Users/yourname/Desktop/project/data.csv')
```

**Good:**

```python
from pathlib import Path

# Works on any computer!
PROJECT_ROOT = Path(__file__).parent.parent
DATA_PATH = PROJECT_ROOT / 'data' / 'raw' / 'data.csv'
data = pd.read_csv(DATA_PATH)
```

### 2. Magic Numbers

**Bad:**

```python
if temperature > 98.6:
    print("Fever")
```

**Good:**

```python
NORMAL_BODY_TEMP_F = 98.6

if temperature > NORMAL_BODY_TEMP_F:
    print("Fever")
```

### 3. Unclear Boolean Logic

**Bad:**

```python
if not (age < 18 or age > 65) and income > 50000:
    eligible = True
```

**Good:**

```python
is_working_age = 18 <= age <= 65
has_sufficient_income = income > 50000

if is_working_age and has_sufficient_income:
    eligible = True
```

### 4. Long Functions

If your function is more than ~50 lines, break it up:

**Bad:**

```python
def analyze_everything(data):
    # 200 lines of code doing many things
    pass
```

**Good:**

```python
def load_data(filepath):
    # 10 lines
    pass

def clean_data(data):
    # 15 lines
    pass

def calculate_statistics(data):
    # 20 lines
    pass

def create_visualizations(statistics):
    # 25 lines
    pass

def analyze_everything(filepath):
    data = load_data(filepath)
    clean = clean_data(data)
    stats = calculate_statistics(clean)
    viz = create_visualizations(stats)
    return stats, viz
```

---

## Your Reproducibility Checklist

Before sharing your code or submitting your paper:

- ✅ Code runs on a fresh environment/computer
- ✅ All file paths are relative, not absolute
- ✅ Random seeds are set for reproducibility
- ✅ Dependencies are documented (requirements.txt or renv)
- ✅ README explains how to run everything
- ✅ Data sources are documented
- ✅ Functions have docstrings
- ✅ Code is organized logically
- ✅ No secrets (API keys, passwords) in code
- ✅ .gitignore excludes sensitive/large files

---

## Template: A Complete Reproducible Script

**Python:**

```python
"""
Customer Churn Analysis
Author: Your Name
Date: 2026-01-26

This script analyzes customer churn patterns and predicts
likelihood of churn based on usage metrics.
"""

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from pathlib import Path
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier

# Configuration
RANDOM_SEED = 42
DATA_PATH = Path('data/raw/customers.csv')
OUTPUT_DIR = Path('outputs')

# Set random seeds
np.random.seed(RANDOM_SEED)

def load_data(filepath):
    """Load and perform initial validation of customer data."""
    data = pd.read_csv(filepath)
    required_columns = ['customer_id', 'usage_hours', 'churned']
    
    if not all(col in data.columns for col in required_columns):
        raise ValueError(f"Missing required columns: {required_columns}")
    
    return data

def main():
    """Main analysis pipeline."""
    # Load
    print("Loading data...")
    data = load_data(DATA_PATH)
    
    # Analyze
    print(f"Analyzing {len(data)} customers...")
    churn_rate = data['churned'].mean()
    print(f"Overall churn rate: {churn_rate:.2%}")
    
    # Model (simplified)
    X = data[['usage_hours']]
    y = data['churned']
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=RANDOM_SEED
    )
    
    model = RandomForestClassifier(random_state=RANDOM_SEED)
    model.fit(X_train, y_train)
    
    score = model.score(X_test, y_test)
    print(f"Model accuracy: {score:.2%}")
    
    # Save results
    OUTPUT_DIR.mkdir(exist_ok=True)
    results = pd.DataFrame({
        'churn_rate': [churn_rate],
        'model_accuracy': [score]
    })
    results.to_csv(OUTPUT_DIR / 'results.csv', index=False)
    print(f"Results saved to {OUTPUT_DIR}")

if __name__ == '__main__':
    main()
```

**R:**
```r
#' Customer Churn Analysis
#' Author: Your Name
#' Date: 2026-01-26
#'
#' This script analyzes customer churn patterns and predicts
#' likelihood of churn based on usage metrics.

library(tidyverse)
library(randomForest)

# Configuration
RANDOM_SEED <- 42
DATA_PATH <- 'data/raw/customers.csv'
OUTPUT_DIR <- 'outputs'

# Set random seed
set.seed(RANDOM_SEED)

#' Load and validate customer data
load_data <- function(filepath) {
  data <- read_csv(filepath)
  required_columns <- c('customer_id', 'usage_hours', 'churned')
  
  if (!all(required_columns %in% names(data))) {
    stop(paste("Missing required columns:", 
               paste(required_columns, collapse = ", ")))
  }
  
  data
}

#' Main analysis pipeline
main <- function() {
  # Load
  cat("Loading data...\n")
  data <- load_data(DATA_PATH)
  
  # Analyze
  cat(sprintf("Analyzing %d customers...\n", nrow(data)))
  churn_rate <- mean(data$churned)
  cat(sprintf("Overall churn rate: %.2f%%\n", churn_rate * 100))
  
  # Model (simplified)
  # Split data
  train_idx <- sample(1:nrow(data), 0.8 * nrow(data))
  train_data <- data[train_idx, ]
  test_data <- data[-train_idx, ]
  
  # Train model
  model <- randomForest(churned ~ usage_hours, 
                       data = train_data,
                       ntree = 100)
  
  # Evaluate
  predictions <- predict(model, test_data)
  accuracy <- mean(predictions == test_data$churned)
  cat(sprintf("Model accuracy: %.2f%%\n", accuracy * 100))
  
  # Save results
  dir.create(OUTPUT_DIR, showWarnings = FALSE, recursive = TRUE)
  results <- tibble(
    churn_rate = churn_rate,
    model_accuracy = accuracy
  )
  write_csv(results, file.path(OUTPUT_DIR, 'results.csv'))
  cat(sprintf("Results saved to %s\n", OUTPUT_DIR))
}

# Run if executed as script
if (!interactive()) {
  main()
}
```

---

## Resources for Learning More

- **"Clean Code" by Robert C. Martin** - Classic programming book
- **"The Pragmatic Programmer"** - Excellent software practices
- **PEP 8** - Python style guide
- **Tidyverse Style Guide** - R style guide
- **"Research Software Engineering with Python"** - Free online book

---

## Next Steps

Now that you can write clean, reproducible code:

1. **[Exploratory Data Analysis](../exploratory-analysis)** - Systematically explore your data
2. **[Data Cleaning Strategies](../data-cleaning)** - Handle messy real-world data
3. **[Working with APIs](../working-with-apis)** - Collect your own data

Remember: Good code is like good writing – it takes practice, but it gets easier. Start with one habit at a time, and soon clean code will be second nature!

*Happy coding!* 💻
