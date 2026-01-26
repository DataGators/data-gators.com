+++
title = "Python Data Analytics Lesson 2"
subtitle = "Exploring Data - Asking Questions and Finding Patterns"
draft = false
+++

# Lesson 2: Exploring Data - Asking Questions and Finding Patterns

## 📘 Overview

Welcome to Lesson 2! In our first lesson, you learned how to load and view data. Now we'll learn how to **explore** that data by asking good questions and looking for interesting patterns.

Think of data exploration like being a detective - you're looking for clues and stories hidden in the numbers!

**Expected Duration:** 30-45 minutes

Required packages:
- pandas
- numpy

## 🎯 Learning Goals

By the end of this lesson, you will be able to:

- Ask different types of questions about your data
- Use pandas functions to explore patterns
- Identify interesting relationships between variables
- Check for data quality issues
- Create a simple data analysis report

## 📌 Setting Up Our Data

Let's start by recreating the student dataset from Lesson 1:

```python
import pandas as pd
import numpy as np

# Create our student dataset
students_data = {
    'Name': ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve', 'Frank', 'Grace', 'Henry', 'Iris', 'Jack'],
    'Grade': ['10th', '11th', '12th', '10th', '12th', '11th', '10th', '12th', '11th', '10th'],
    'Math_Score': [85, 76, 92, 88, 79, 84, 91, 87, 83, 90],
    'Science_Score': [78, 82, 89, 85, 88, 79, 86, 91, 87, 84],
    'English_Score': [92, 85, 88, 90, 86, 88, 94, 89, 91, 87],
    'Hours_Studied': [5, 8, 6, 7, 4, 9, 5, 8, 6, 7],
    'Extracurriculars': [2, 1, 3, 2, 1, 2, 4, 1, 3, 2]
}

df = pd.DataFrame(students_data)
print("Our Student Dataset:")
print(df)
```

## 🔍 Understanding Different Types of Questions

When exploring data, we can ask three main types of questions:

1. **Descriptive Questions**: "What happened?" or "What do we have?"
2. **Comparative Questions**: "How do different groups compare?"
3. **Relationship Questions**: "How are different things connected?"

Let's practice each type!

## 1. Descriptive Questions

These help us understand the basic characteristics of our data:

```python
# Question: How many students do we have?
print(f"Total number of students: {len(df)}")
```

```python
# Question: What's the average score in each subject?
print("Average Scores:")
print(f"Math: {df['Math_Score'].mean():.1f}")
print(f"Science: {df['Science_Score'].mean():.1f}")
print(f"English: {df['English_Score'].mean():.1f}")
```

```python
# Question: What's the range of study hours?
print(f"Study Hours - Min: {df['Hours_Studied'].min()}, Max: {df['Hours_Studied'].max()}")
print(f"Average study hours: {df['Hours_Studied'].mean():.1f}")
```

```python
# Question: How many students are in each grade?
print("Students by Grade:")
print(df['Grade'].value_counts())
```

## 2. Comparative Questions

These help us compare different groups:

```python
# Question: Do older students score better than younger students?
grade_performance = df.groupby('Grade')[['Math_Score', 'Science_Score', 'English_Score']].mean()
print("Average Scores by Grade:")
print(grade_performance.round(1))
```

```python
# Question: Who studies the most hours by grade?
study_by_grade = df.groupby('Grade')['Hours_Studied'].mean()
print("Average Study Hours by Grade:")
print(study_by_grade.round(1))
```

```python
# Question: Which subject has the highest scores?
subject_averages = {
    'Math': df['Math_Score'].mean(),
    'Science': df['Science_Score'].mean(), 
    'English': df['English_Score'].mean()
}

print("Subject Rankings (highest to lowest average):")
sorted_subjects = sorted(subject_averages.items(), key=lambda x: x[1], reverse=True)
for i, (subject, avg) in enumerate(sorted_subjects, 1):
    print(f"{i}. {subject}: {avg:.1f}")
```

## 3. Relationship Questions

These help us understand how different variables might be connected:

```python
# Question: Do students who study more hours get better grades?
# Let's calculate total score first
df['Total_Score'] = df['Math_Score'] + df['Science_Score'] + df['English_Score']

print("Study Hours vs. Total Scores:")
for _, student in df.iterrows():
    print(f"{student['Name']}: {student['Hours_Studied']} hours → {student['Total_Score']} total points")
```

```python
# Question: Are students with more extracurriculars better at certain subjects?
print("Extracurriculars vs. English Scores (communication skills):")
for _, student in df.iterrows():
    print(f"{student['Name']}: {student['Extracurriculars']} activities → {student['English_Score']} English")
```

## 🔎 Finding Interesting Patterns

Let's look for patterns that might surprise us:

```python
# Create some interesting comparisons
df['Average_Score'] = df[['Math_Score', 'Science_Score', 'English_Score']].mean(axis=1)

# Question: Who are the highest and lowest performers?
best_student = df.loc[df['Average_Score'].idxmax()]
struggling_student = df.loc[df['Average_Score'].idxmin()]

print("Highest Performer:")
print(f"{best_student['Name']} (Grade {best_student['Grade']}): {best_student['Average_Score']:.1f} average")
print(f"Studies {best_student['Hours_Studied']} hours/week")

print("\nNeeds More Support:")
print(f"{struggling_student['Name']} (Grade {struggling_student['Grade']}): {struggling_student['Average_Score']:.1f} average")
print(f"Studies {struggling_student['Hours_Studied']} hours/week")
```

```python
# Question: Is there a "sweet spot" for extracurricular activities?
print("Extracurriculars vs Performance:")
activity_performance = df.groupby('Extracurriculars')['Average_Score'].mean()
for activities, avg_score in activity_performance.items():
    print(f"{activities} activities: {avg_score:.1f} average score")
```

## 🛠️ Identifying Data Quality Issues

Part of exploration is checking if our data makes sense:

```python
# Check for outliers (unusually high or low values)
print("Checking for Unusual Values:")

# Are any scores impossible? (over 100 or under 0)
for subject in ['Math_Score', 'Science_Score', 'English_Score']:
    max_score = df[subject].max()
    min_score = df[subject].min()
    print(f"{subject}: Range {min_score} to {max_score}")
    if max_score > 100 or min_score < 0:
        print(f"  ⚠️  Warning: Unusual scores in {subject}")

# Are study hours realistic?
max_hours = df['Hours_Studied'].max()
if max_hours > 20:
    print(f"⚠️  Warning: {max_hours} study hours per week seems very high")
else:
    print(f"✓ Study hours look reasonable (max: {max_hours} hours/week)")
```

## 💡 Generating New Questions from Exploration

Based on what we've discovered, we can ask deeper questions:

```python
# New insights lead to new questions
print("New Questions to Investigate:")
print("\n1. Effect of Study Time:")
high_study = df[df['Hours_Studied'] >= 7]['Average_Score'].mean()
low_study = df[df['Hours_Studied'] < 7]['Average_Score'].mean()
print(f"   High study time (7+ hours): {high_study:.1f} average")
print(f"   Low study time (<7 hours): {low_study:.1f} average")
print(f"   Difference: {high_study - low_study:.1f} points")
```

```python
print("2. Grade Level Performance:")
grade_12_avg = df[df['Grade'] == '12th']['Average_Score'].mean()
grade_10_avg = df[df['Grade'] == '10th']['Average_Score'].mean()
print(f"   12th grade average: {grade_12_avg:.1f}")
print(f"   10th grade average: {grade_10_avg:.1f}")
print(f"   Improvement from 10th to 12th: {grade_12_avg - grade_10_avg:.1f} points")
```

## 📊 Creating a Data Summary Report

Let's create a simple report of what we learned:

```python
print("=" * 50)
print("STUDENT PERFORMANCE ANALYSIS REPORT")
print("=" * 50)

print(f"\nDataset Overview:")
print(f"• Total students: {len(df)}")
print(f"• Grade levels: {', '.join(sorted(df['Grade'].unique()))}")
print(f"• Subjects analyzed: Math, Science, English")

print(f"\nKey Findings:")
print(f"• Best subject overall: English ({df['English_Score'].mean():.1f} average)")
print(f"• Most challenging subject: Math ({df['Math_Score'].mean():.1f} average)")
print(f"• Study time range: {df['Hours_Studied'].min()}-{df['Hours_Studied'].max()} hours per week")

print(f"\nInteresting Patterns:")
correlation = df['Hours_Studied'].corr(df['Average_Score'])
if correlation > 0.5:
    print(f"• Strong positive link between study time and performance")
elif correlation > 0:
    print(f"• Weak positive link between study time and performance")
else:
    print(f"• No clear link between study time and performance")

print(f"\nQuestions for Further Investigation:")
print(f"• Why do some students perform well with less study time?")
print(f"• What teaching methods work best for different subjects?")
print(f"• How do extracurricular activities affect academic performance?")
```

## ✅ Key Learning Points

1. **Start with simple questions** before moving to complex ones
2. **Look for patterns and outliers** - they often tell interesting stories
3. **Always check if your data makes sense** - impossible values indicate problems
4. **Group and compare** different segments of your data
5. **New discoveries lead to new questions** - that's the beauty of data exploration!

## 💡 Practice Exercises

Try these exercises to practice your data exploration skills:

### Exercise 1: Create Your Own Analysis

Using the movie dataset from last lesson, explore:
- What's the average rating by decade?
- Which genres are most common?
- Are newer movies rated higher or lower?

```python
# First, let's recreate the movie dataset:
movies_data = {
    'Title': ['The Matrix', 'Inception', 'Parasite', 'Toy Story', 'Pulp Fiction', 'The Dark Knight'],
    'Year': [1999, 2010, 2019, 1995, 1994, 2008],
    'Genre': ['Sci-Fi', 'Sci-Fi', 'Thriller', 'Animation', 'Crime', 'Action'],
    'Rating': [8.7, 8.8, 8.6, 8.3, 8.9, 9.0],
    'Duration': [136, 148, 132, 81, 154, 152]
}

movies_df = pd.DataFrame(movies_data)
print("Movie Dataset:")
print(movies_df)

# Your analysis code here...
```

### Exercise 2: Generate Questions

Look at your movie data and write down 5 questions you could answer. Try asking questions about ratings and years, duration and genre, or patterns over time.

### Exercise 3: Find Patterns

Group your movies by genre and compare their ratings.

### Exercise 4: Quality Check

Look for any unusual values in your movie dataset. Check if ratings are between 0-10, years make sense, etc.

## 🚀 What's Next?

Now that we know how to explore data and ask good questions, we're ready to create our first visualizations! In the next lesson, we'll learn how to make charts and graphs that help us see patterns more clearly.

## 📓 Interactive Notebook

Want to practice with the interactive Jupyter notebook version of this lesson?

[Download the Jupyter Notebook](/files/my_analysis_project/data-analytics-02-exploring-data.ipynb)
