# Data Science Tutorials for Young Researchers

This directory contains a comprehensive collection of tutorials designed to help young researchers master data science from project inception to publication.

## Directory Structure

```
contents/
└── tutorials/
    ├── _index.md                      # Main landing page with overview
    ├── setup-environment.md           # Installing Python, R, Jupyter, VS Code, Git
    ├── reproducible-code.md           # Writing clean, reproducible research code
    ├── exploratory-analysis.md        # Systematic EDA framework
    ├── data-cleaning.md               # Handling missing values, outliers, inconsistencies
    ├── working-with-apis.md           # Collecting data from APIs
    ├── data-visualization.md          # Creating publication-quality visualizations
    ├── research-ethics.md             # Privacy, bias, fairness, responsible practices
    └── research-portfolio.md          # Building your online research presence
```

## Using These Tutorials

### For Hugo Sites

1. **Copy this entire `tutorials/` directory** into your Hugo site's `content/` directory:
   ```bash
   cp -r contents/tutorials/ /path/to/your/hugo/site/content/
   ```

2. **Verify your Hugo configuration** includes markdown rendering:
   ```toml
   # config.toml
   [markup]
     [markup.goldmark]
       [markup.goldmark.renderer]
         unsafe = true  # Allows HTML in markdown
   ```

3. **Build and serve**:
   ```bash
   cd /path/to/your/hugo/site
   hugo server -D
   ```

4. **Access tutorials** at: `http://localhost:1313/tutorials/`

### Tutorial Weights

Tutorials are ordered by the `weight` parameter in their front matter:
- Lower numbers appear first
- Suggested learning path follows numerical order
- Can be customized per your needs

### Customization

Each tutorial includes:
- **Front matter** for Hugo metadata
- **Code examples** in both Python and R
- **Practical exercises** and examples
- **Links** to related tutorials
- **Resource lists** for further learning

Feel free to:
- Modify content for your specific audience
- Add institution-specific resources
- Include your own examples
- Adjust difficulty levels

## Tutorial Overview

### Getting Started (Weights 1-3)
Essential setup and foundational skills for beginning researchers.

**1. Setting Up Your Data Science Environment**
- Installing Python, R, Jupyter, VS Code
- Virtual environments and package management
- Git basics and GitHub setup

**2. Writing Clean, Reproducible Research Code**
- Project organization and structure
- Functions, documentation, and testing
- Version control with Git

**3. Exploratory Data Analysis**
- Systematic data exploration framework
- Univariate, bivariate, multivariate analysis
- Visualization and hypothesis generation

### Working with Data (Weights 4-5)
Collecting and preparing data for analysis.

**4. Data Cleaning Strategies**
- Missing value handling strategies
- Outlier detection and treatment
- Data validation and consistency checks

**5. Working with APIs**
- API basics and authentication
- Collecting data from social media, weather, financial APIs
- Rate limiting and error handling

### Communication (Weights 10-11)
Sharing your research effectively and ethically.

**10. Data Visualization**
- Choosing appropriate chart types
- Python (matplotlib, seaborn) and R (ggplot2)
- Publication-quality figures

**11. Research Ethics**
- Privacy and data protection
- Bias and fairness in algorithms
- IRB approval and responsible practices

### Professional Development (Weight 15)
Building your research career.

**15. Building a Research Portfolio**
- GitHub profile optimization
- Personal website creation
- Project documentation best practices

## Additional Tutorials Available

While not yet created in this directory, the index page references these additional topics. You can create them as needed:

- Web Scraping Ethics and Techniques
- Statistical Testing for Researchers
- Feature Engineering
- Model Selection and Evaluation
- Time Series Analysis
- Writing Your First Research Paper
- Managing Large Datasets
- Debugging Your Analysis
- Collaborating on Research Projects
- From Jupyter Notebook to Production Code

## Contributing

To add or modify tutorials:

1. **Follow the existing format**:
   - Use the same front matter structure
   - Include both Python and R examples where appropriate
   - Maintain a friendly, accessible tone
   - Add practical, runnable code examples

2. **Naming conventions**:
   - Use lowercase with hyphens: `my-tutorial.md`
   - Keep filenames descriptive but concise

3. **Weight assignment**:
   - Group related tutorials with similar weights
   - Leave gaps (e.g., 1, 2, 3, 10, 11) to allow for insertions

4. **Cross-linking**:
   - Link to prerequisite tutorials
   - Suggest next steps at the end
   - Use relative links: `[Tutorial Name](../tutorial-name)`

## Technical Requirements

### For Hugo Sites

**Minimum Hugo version**: 0.80.0 or later

**Required Hugo configuration**:
```toml
[markup]
  [markup.highlight]
    codeFences = true
    lineNos = false
    style = "monokai"
```

### Code Blocks

Tutorials use fenced code blocks with language specifications:
- ` ```python ` for Python code
- ` ```r ` for R code
- ` ```bash ` for shell commands
- ` ```markdown ` for markdown examples

### Images

If adding images:
1. Place in `static/images/tutorials/`
2. Reference as: `![Alt text](/images/tutorials/image-name.png)`

## Support

For issues or questions:
- Check the individual tutorial for specific guidance
- Review the main index page for navigation
- Consult Hugo documentation: https://gohugo.io/documentation/

## License

These tutorials are designed for educational use. Please maintain attribution when adapting or redistributing.

## Changelog

- **2026-01-26**: Initial creation of tutorial collection
  - 8 core tutorials created
  - Comprehensive index page
  - Hugo-ready markdown format

---

**For Students**: Start with the [Tutorial Index](_index.md) to find your way!

**For Instructors**: Feel free to adapt these tutorials for your courses and workshops.

*Happy learning and researching!* 🎓🔬
