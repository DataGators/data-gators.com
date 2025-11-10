+++
title = "Python Data Analytics Lesson 5"
subtitle = "Discovering Relationships with Correlations"
draft = false
+++

# Lesson 5: Discovering Relationships with Correlations

## 📘 Overview

Correlation helps us discover if two things tend to change together. Do students who study more get better grades? Are older students better at math? Today we'll learn how to find these hidden relationships in our data!

**Expected Duration:** 45-60 minutes

Required packages:
- pandas
- numpy
- matplotlib
- seaborn

## 🎯 Learning Goals

By the end of this lesson, you will be able to:

- Calculate and interpret correlation coefficients
- Create and read correlation matrices
- Visualize relationships with scatter plots and heatmaps
- Understand the difference between correlation and causation
- Tell compelling stories with correlation analysis

## 📌 Setting Up Our Enhanced Dataset

Let's expand our student dataset with more variables to explore relationships:

```python
# Install required packages for data analytics
import piplite
await piplite.install(['seaborn', 'matplotlib', 'pandas', 'numpy', 'scipy', 'plotly'])
print("Packages installed successfully!")
print("You can now import and use: seaborn, matplotlib, pandas, numpy, scipy, plotly")
```

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Enhanced student dataset with more variables for correlation analysis
student_data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eva', 'Frank', 'Grace', 'Henry'],
    'Age': [16, 17, 16, 18, 17, 16, 17, 18],
    'Grade': ['10th', '11th', '10th', '12th', '11th', '10th', '11th', '12th'],
    'Math_Score': [85, 92, 78, 95, 88, 76, 91, 87],
    'Science_Score': [89, 87, 82, 98, 85, 79, 93, 89],
    'English_Score': [92, 85, 88, 91, 94, 83, 89, 86],
    'Hours_Studied': [5, 8, 4, 10, 6, 3, 9, 7],
    'Extracurriculars': [2, 1, 3, 2, 4, 1, 2, 3],
    'Sleep_Hours': [7, 6, 8, 6, 7, 9, 6, 7],         # New: hours of sleep per night
    'Screen_Time': [4, 6, 3, 2, 5, 7, 3, 4],          # New: recreational screen hours per day
    'Books_Read': [12, 8, 15, 20, 10, 5, 18, 14]      # New: books read per year
}

df = pd.DataFrame(student_data)
df['Average_Score'] = df[['Math_Score', 'Science_Score', 'English_Score']].mean(axis=1)

print("Enhanced Student Dataset for Correlation Analysis:")
print(df[['Name', 'Hours_Studied', 'Average_Score', 'Sleep_Hours', 'Screen_Time', 'Books_Read']])
```

## 🔍 Understanding Correlation

Correlation measures how strongly two variables are related on a scale from -1 to +1:

```python
print("UNDERSTANDING CORRELATION COEFFICIENTS:")
print("┌─────────────────┬────────────────────────────────────┐")
print("│ Correlation     │ What it means                      │")
print("├─────────────────┼────────────────────────────────────┤")
print("│ +1.0            │ Perfect positive relationship      │")
print("│ +0.7 to +0.9    │ Strong positive relationship       │")
print("│ +0.3 to +0.7    │ Moderate positive relationship     │") 
print("│ +0.1 to +0.3    │ Weak positive relationship         │")
print("│ 0.0             │ No relationship                    │")
print("│ -0.1 to -0.3    │ Weak negative relationship         │")
print("│ -0.3 to -0.7    │ Moderate negative relationship     │")
print("│ -0.7 to -0.9    │ Strong negative relationship       │")
print("│ -1.0            │ Perfect negative relationship      │")
print("└─────────────────┴────────────────────────────────────┘")

print("\nPositive correlation: As one goes up, the other goes up")
print("Negative correlation: As one goes up, the other goes down")
print("No correlation: The variables are independent of each other")
```

## 1. Single Correlation Analysis

Let's start by examining specific relationships:

```python
# Do students who study more get better grades?
study_grade_corr = df['Hours_Studied'].corr(df['Average_Score'])

print("RELATIONSHIP: Study Hours vs. Average Score")
print(f"Correlation coefficient: {study_grade_corr:.3f}")

# Interpret the correlation
if study_grade_corr > 0.7:
    interpretation = "Strong positive - More study time strongly linked to better grades!"
elif study_grade_corr > 0.3:
    interpretation = "Moderate positive - More study time somewhat linked to better grades"
elif study_grade_corr > 0.1:
    interpretation = "Weak positive - Slight tendency for more study to help grades"
elif study_grade_corr > -0.1:
    interpretation = "No clear relationship between study time and grades"
else:
    interpretation = "Negative relationship - This would be surprising!"

print(f"Interpretation: {interpretation}")
```

## 2. Correlation Matrix - See All Relationships at Once

A correlation matrix shows the correlation between every pair of variables:

```python
# Create a correlation matrix for all numeric variables
numeric_columns = ['Age', 'Math_Score', 'Science_Score', 'English_Score', 
                  'Hours_Studied', 'Extracurriculars', 'Sleep_Hours', 
                  'Screen_Time', 'Books_Read', 'Average_Score']

correlation_matrix = df[numeric_columns].corr()

print("CORRELATION MATRIX:")
print("(Each number shows correlation between row and column variables)")
print(correlation_matrix.round(3))
```

## 3. Visualizing Correlations with Heatmaps

```python
# Create a heatmap to visualize the correlation matrix
plt.figure(figsize=(12, 10))
mask = np.triu(np.ones_like(correlation_matrix))  # Hide upper triangle (it's redundant)

# Create the heatmap
sns.heatmap(correlation_matrix, 
            annot=True,          # Show correlation numbers
            cmap='RdBu_r',       # Red-Blue color scheme
            center=0,            # Center the color map at 0
            mask=mask,           # Apply the mask
            square=True,         # Make cells square
            fmt='.2f',           # Format numbers to 2 decimal places
            cbar_kws={'label': 'Correlation Coefficient'})

plt.title('Student Data Correlation Matrix', fontsize=16, fontweight='bold', pad=20)
plt.tight_layout()
plt.show()
```

## 4. Creating a Correlation Story

Let's create a narrative about our findings:

```python
# Create subplots showing our key correlations
fig, axes = plt.subplots(2, 2, figsize=(15, 12))
fig.suptitle('Key Correlation Findings', fontsize=16, fontweight='bold')

# 1. Study Hours vs Performance
axes[0, 0].scatter(df['Hours_Studied'], df['Average_Score'], color='blue', s=100, alpha=0.7)
z1 = np.polyfit(df['Hours_Studied'], df['Average_Score'], 1)
p1 = np.poly1d(z1)
axes[0, 0].plot(df['Hours_Studied'], p1(df['Hours_Studied']), "r--", alpha=0.8)
corr1 = df['Hours_Studied'].corr(df['Average_Score'])
axes[0, 0].set_title(f'Study Hours vs Performance\n(r = {corr1:.3f})')
axes[0, 0].set_xlabel('Hours Studied')
axes[0, 0].set_ylabel('Average Score')

# 2. Sleep vs Performance  
axes[0, 1].scatter(df['Sleep_Hours'], df['Average_Score'], color='green', s=100, alpha=0.7)
z2 = np.polyfit(df['Sleep_Hours'], df['Average_Score'], 1)
p2 = np.poly1d(z2)
axes[0, 1].plot(df['Sleep_Hours'], p2(df['Sleep_Hours']), "r--", alpha=0.8)
corr2 = df['Sleep_Hours'].corr(df['Average_Score'])
axes[0, 1].set_title(f'Sleep Hours vs Performance\n(r = {corr2:.3f})')
axes[0, 1].set_xlabel('Sleep Hours')
axes[0, 1].set_ylabel('Average Score')

# 3. Screen Time vs Performance
axes[1, 0].scatter(df['Screen_Time'], df['Average_Score'], color='red', s=100, alpha=0.7)
z3 = np.polyfit(df['Screen_Time'], df['Average_Score'], 1)
p3 = np.poly1d(z3)
axes[1, 0].plot(df['Screen_Time'], p3(df['Screen_Time']), "r--", alpha=0.8)
corr3 = df['Screen_Time'].corr(df['Average_Score'])
axes[1, 0].set_title(f'Screen Time vs Performance\n(r = {corr3:.3f})')
axes[1, 0].set_xlabel('Screen Hours')
axes[1, 0].set_ylabel('Average Score')

# 4. Books vs English
axes[1, 1].scatter(df['Books_Read'], df['English_Score'], color='purple', s=100, alpha=0.7)
z4 = np.polyfit(df['Books_Read'], df['English_Score'], 1)
p4 = np.poly1d(z4)
axes[1, 1].plot(df['Books_Read'], p4(df['Books_Read']), "r--", alpha=0.8)
corr4 = df['Books_Read'].corr(df['English_Score'])
axes[1, 1].set_title(f'Books Read vs English Score\n(r = {corr4:.3f})')
axes[1, 1].set_xlabel('Books per Year')
axes[1, 1].set_ylabel('English Score')

plt.tight_layout()
plt.show()
```

## ⚠️ Correlation vs. Causation Warning

**This is crucial to understand:**

```python
print("⚠️  IMPORTANT: CORRELATION ≠ CAUSATION ⚠️")
print("="*50)
print()
print("Just because two things are correlated doesn't mean one CAUSES the other!")
print()
print("Examples from our data:")

# Find a strong correlation
books_english_corr = df['Books_Read'].corr(df['English_Score'])
print(f"Books Read ↔ English Score: {books_english_corr:.3f}")
print()
print("Possible explanations:")
print("1. 📚 Reading more books improves English skills (books → English)")
print("2. 🎓 Students good at English enjoy reading more (English → books)")  
print("3. 👨‍👩‍👧‍👦 Family values education (causes both reading AND good English)")
print("4. 💰 Family income affects both book access AND tutoring")
print()

print("How to think about correlation:")
print("• Correlation suggests a relationship exists")
print("• It helps us ask better questions")  
print("• We need experiments or more data to prove causation")
print("• Always consider alternative explanations")
```

## ✅ Key Learning Points

1. **Correlation measures relationship strength** from -1 to +1
2. **Positive correlation**: Both variables increase together
3. **Negative correlation**: One increases as the other decreases
4. **Correlation matrices** show all relationships at once
5. **Correlation ≠ Causation** - always consider alternative explanations
6. **Strong correlations** (>0.7 or <-0.7) suggest important relationships worth investigating

## 💡 Practice Exercises

Try these exercises to practice your correlation analysis skills. Download the interactive notebook to work through them!

## 🚀 What's Next?

Now that we can find relationships in our data, we're ready to create more advanced visualizations and dive into statistical significance testing! In the next lesson, we'll explore advanced plotting techniques and learn about p-values and t-tests.

## 📓 Interactive Notebook

Want to practice with the interactive Jupyter notebook version of this lesson?

[Download the Jupyter Notebook](/files/my_analysis_project/data-analytics-05-correlations.ipynb)
