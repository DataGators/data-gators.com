# Data Gators Content Creation Workflow

This document serves as a comprehensive guide for Data Gators mentors to create, organize, and maintain educational content on the Data Gators website. Following these workflows will ensure consistency across lessons and workshops.

## Table of Contents
- [Data Gators Content Creation Workflow](#data-gators-content-creation-workflow)
  - [Table of Contents](#table-of-contents)
  - [Website Architecture](#website-architecture)
  - [Content Creation Process](#content-creation-process)
  - [Hugo Content Structure](#hugo-content-structure)
    - [File Naming Conventions](#file-naming-conventions)
  - [Lesson Template](#lesson-template)
    - [Hugo Shortcodes \& Features](#hugo-shortcodes--features)
  - [Google Colab Integration](#google-colab-integration)
    - [Setting Up a New Colab Notebook](#setting-up-a-new-colab-notebook)
    - [Creating Effective Colab Notebooks](#creating-effective-colab-notebooks)
    - [Example Notebook Structure](#example-notebook-structure)
    - [Testing Notebook Accessibility](#testing-notebook-accessibility)
  - [Workshop Preparation](#workshop-preparation)
  - [Publishing Workflow](#publishing-workflow)
    - [Local Testing](#local-testing)
  - [Reference Resources](#reference-resources)

## Website Architecture

The Data Gators website is built with [Hugo](https://gohugo.io/), a static site generator that allows for fast and flexible content creation. We use the [Relearn theme](https://mcshelby.github.io/hugo-theme-relearn/) for its documentation-friendly features.

Key directories:
- `content/` - Contains all website content organized by section
- `static/` - Houses static assets like images
- `themes/` - Contains the Hugo theme files
- `config.toml` - Main configuration file for the site

## Content Creation Process

1. **Plan content** - Decide on topic, difficulty level, required packages, and learning objectives
2. **Create lesson page** - Write lesson content in Markdown following our template
3. **Develop Google Colab notebook** - Create interactive notebook that complements the lesson
4. **Add supporting materials** - Prepare datasets, images, and reference materials
5. **Review & test** - Ensure all code runs correctly and content flows logically
6. **Publish** - Add to website repository and deploy

## Hugo Content Structure

Hugo organizes content into sections. Our main sections are:

- `lessons/` - Step-by-step tutorials on data science topics
- `resources/` - Reference materials and cheat sheets
- `tutorials/` - More advanced or specialized topics
- `events/` - Information about workshops and events
- `about/` - Information about Data Gators

### File Naming Conventions

- Use lowercase with underscores: `lesson_2.md` not `Lesson2.md`
- Number sequential content: `lesson_1.md`, `lesson_2.md`, etc.
- Include language if applicable: `lesson_2_python.md` or `lesson_2_r.md`

## Lesson Template

Each lesson should follow this structure:

```
+++
title = "Lesson Title"
subtitle = "Descriptive Subtitle"
weight = 10  # Controls ordering in navigation
draft = false  # Set to true if not ready to publish
+++

# Lesson Title

## 📘 Overview

Brief introduction to the topic and its importance in data science.
Include practical applications and expected learning outcomes.

{{% notice info %}}
**Expected Duration:** XX minutes  
**Prerequisites:** Any required knowledge
**Difficulty Level:** Beginner/Intermediate/Advanced

Required packages:
- package1
- package2
{{% /notice %}}

## 📦 Package Installation

Code block for installation (use single backticks for actual implementation):

`r
# Installation code here
`

## 📌 Dataset Overview

Description of the dataset used in this lesson.
Include key variables and their meanings.

Code block for dataset exploration:

`r
# Code to explore dataset
`

## 🛠 Main Content Sections

### 1. First Topic

Explanation and code examples

Code block for first topic:

`r
# Code block
`

### 2. Second Topic

Explanation and code examples

Code block for second topic:

`r
# Code block
`

## ✅ Key Takeaways

- Bullet points summarizing main lessons
- Focus on practical skills gained

## 🚀 Practice Exercise

Try the code yourself in our interactive notebook:

{{< button href="COLAB_LINK_HERE" >}}Open in Google Colab{{< /button >}}

{{% notice info %}}
**Important:** After clicking the link above, click the "Copy to Drive" button in Colab to create your own editable copy of the notebook.
{{% /notice %}}
```

> **Note**: When implementing the actual lesson page, replace the single backticks shown above with triple backticks for proper code blocks.

### Hugo Shortcodes & Features

Our theme provides several useful shortcodes to enhance content:

1. **Notice boxes** - For highlighting important information:
   ```
   {{% notice info %}}
   Important information here
   {{% /notice %}}
   ```

   Other notice types: `note`, `tip`, `warning`, `danger`

2. **Buttons** - For clear call-to-actions:
   ```
   {{< button href="URL" >}}Button Text{{< /button >}}
   ```

3. **Tabs** - For organizing content:
   ```
   {{< tabs >}}
   {{< tab name="R" >}}R code here{{< /tab >}}
   {{< tab name="Python" >}}Python code here{{< /tab >}}
   {{< tabs >}}
   ```

4. **Expandable sections**:
   ```
   {{< expand "Click to expand" >}}
   Hidden content here
   {{< /expand >}}
   ```

For more shortcodes and features, refer to the [Hugo Relearn Theme documentation](https://mcshelby.github.io/hugo-theme-relearn/shortcodes/index.html).

## Google Colab Integration

Every lesson should have a corresponding Google Colab notebook to provide hands-on practice.

### Setting Up a New Colab Notebook

1. **Create a new notebook**:
   - Go to [Google Colab](https://colab.research.google.com/)
   - Click "New Notebook"
   - Immediately save to your Google Drive

2. **Set the correct language** (very important):
   - Click on "Edit" in the menu bar
   - Select "Notebook settings"
   - In the dropdown for "Runtime type", select either:
     - "Python" for Python notebooks
     - "R" for R notebooks
   - Click "Save"

3. **Configure sharing settings** (critical):
   - Click the "Share" button in the top right corner
   - Set to "Anyone with the link" 
   - Set permission level to "Viewer" only (NOT "Editor")
   - This prevents users from modifying your original notebook
   - Copy the link and add `#forceEdit=true&sandboxMode=true` to the end
   - This ensures users are prompted to make their own copy

4. **Add clear instructions for students**:
   - Include a prominent cell at the top of the notebook with:
   ```
   # IMPORTANT: Click "Copy to Drive" button above to create your own editable copy
   # Do not request edit access to this original notebook
   ```

### Creating Effective Colab Notebooks

1. **Set up the notebook**:
   - Include title and overview at the top
   - Add installation cell for required packages
   - Organize with clear section headings
   - Use markdown cells to explain concepts
   - Include both complete examples and exercises with blanks for students to fill in

2. **Structure your notebook**:
   - Start with easier tasks and progress to more complex ones
   - Provide sample solutions in hidden cells or separate sections
   - Include visualization cells to help students verify their work
   - End with challenge problems for advanced students

3. **Make it interactive**:
   - Include broken functions for students to fix
   - Create cells with `# TODO` comments for students to complete
   - Add validation cells that check if student solutions work correctly

### Example Notebook Structure

For Python:
```
# Title and Overview (markdown cell)

# IMPORTANT: Click "Copy to Drive" button above to create your own editable copy
# Do not request edit access to this original notebook

# Installation Cell (code cell)
!pip install pandas matplotlib seaborn

# Data Loading (code cell)
import pandas as pd
data = pd.read_csv('https://raw.githubusercontent.com/path/to/dataset.csv')

# Exploration Section (mix of markdown and code)
# Exercise 1: Complete this function (code cell with gaps)
def clean_data(df):
    # TODO: Remove duplicates
    # TODO: Handle missing values
    return df

# Solution Validation (code cell)
# Advanced Challenges (code cells with difficult problems)
```

For R:
```
# Title and Overview (markdown cell)

# IMPORTANT: Click "Copy to Drive" button above to create your own editable copy
# Do not request edit access to this original notebook

# Installation Cell (code cell)
install.packages(c("dplyr", "ggplot2", "tidyr"))
library(dplyr)
library(ggplot2)
library(tidyr)

# Data Loading (code cell)
data <- read.csv("https://raw.githubusercontent.com/path/to/dataset.csv")

# Exploration Section (mix of markdown and code)
# Exercise 1: Complete this function (code cell with gaps)
clean_data <- function(df) {
  # TODO: Remove duplicates
  # TODO: Handle missing values
  return(df)
}

# Solution Validation (code cell)
# Advanced Challenges (code cells with difficult problems)
```

### Testing Notebook Accessibility

Before finalizing your notebook:
1. Open your shared link in an incognito/private browsing window
2. Verify that the "Copy to Drive" button appears
3. Test that all datasets and resources are accessible without authentication
4. Check that code runs correctly in a fresh environment

## Workshop Preparation

When preparing for a live workshop:

1. **Create a shortened URL** for the Colab notebook using bit.ly or similar
2. **Prepare a slide deck** introducing key concepts
3. **Test all code** on different environments
4. **Create a worksheet** with additional exercises
5. **Prepare a feedback form** to collect student responses
6. **Create a QR code** linking to the Colab notebook for easy access

## Publishing Workflow

1. **Prepare content locally** following the templates above
2. **Test all code** to ensure it runs without errors
3. **Create pull request** to the Data Gators GitHub repository
4. **Request review** from another mentor
5. **Deploy changes** once approved

### Local Testing

To test your content locally before publishing:

```bash
# Install Hugo
# Clone repository
git clone https://github.com/your-org/data-gators.com.git
cd data-gators.com

# Start local server
hugo server -D
```

View your site at http://localhost:1313/

## Reference Resources

- [Hugo Documentation](https://gohugo.io/documentation/)
- [Hugo Relearn Theme Documentation](https://mcshelby.github.io/hugo-theme-relearn/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Google Colab Documentation](https://colab.research.google.com/notebooks/basic_features_overview.ipynb)
- [Google Colab for R](https://colab.research.google.com/notebook#create=true&language=r)
