---
title: "Research Ethics in Data Science"
date: 2026-01-26
draft: false
description: "Understanding privacy, bias, fairness, and responsible practices in data science research"
tags: ["ethics", "privacy", "bias", "fairness", "responsible-AI"]
categories: ["Ethics", "Best Practices"]
weight: 11
---

## Why Ethics Matters in Data Science

Data science has real-world impact. Your models and analyses affect people's lives - from loan approvals to medical diagnoses to criminal justice. As researchers, we have a responsibility to do no harm.

**Remember:** Just because you *can* do something with data doesn't mean you *should*.

---

## Core Ethical Principles

### 1. **Privacy**: Protect Personal Information

**What to consider:**
- Is this data about identifiable people?
- Do subjects know their data is being used this way?
- Could re-identification be possible?
- Are you storing data securely?

**Best practices:**
```python
# Remove direct identifiers
data = data.drop(['name', 'ssn', 'email', 'phone'], axis=1)

# Generalize quasi-identifiers
data['age_group'] = pd.cut(data['age'], bins=[0, 18, 30, 50, 100])
data = data.drop('age', axis=1)

# Add noise for privacy (differential privacy)
from diffprivlib import tools
private_mean = tools.mean(data['income'], epsilon=1.0, bounds=(0, 1000000))
```

### 2. **Consent**: Use Data Appropriately

**Questions to ask:**
- Did people consent to this specific use?
- Is this within the scope of the original purpose?
- Are you violating any terms of service?

**Example - Twitter data:**
- ✅ Aggregate analysis of public tweets
- ✅ Studying trends and patterns
- ❌ Publishing usernames with controversial tweets
- ❌ Identifying individuals without consent

### 3. **Bias**: Recognize and Mitigate Unfairness

**Types of bias:**
- **Selection bias**: Non-representative sampling
- **Measurement bias**: Systematic errors in data collection
- **Algorithmic bias**: Models that discriminate
- **Confirmation bias**: Seeing what you want to see

**Checking for bias:**
```python
# Check representation across groups
print(data.groupby(['gender', 'race']).size())

# Check model performance across groups
from sklearn.metrics import accuracy_score

for group in data['demographic_group'].unique():
    group_data = data[data['demographic_group'] == group]
    accuracy = accuracy_score(group_data['true_label'], 
                             group_data['predicted_label'])
    print(f"{group}: {accuracy:.3f}")

# Fairness metrics
from fairlearn.metrics import demographic_parity_ratio
dpr = demographic_parity_ratio(y_true, y_pred, sensitive_features=sensitive)
print(f"Demographic parity ratio: {dpr:.3f}")  # Should be close to 1.0
```

### 4. **Transparency**: Be Open About Your Methods

**What to document:**
- Data sources and collection methods
- Preprocessing steps and decisions
- Model choices and hyperparameters
- Known limitations
- Potential biases

**Example documentation:**
```markdown
## Data Source
Customer transaction data from XYZ Corp (2020-2025)
- n = 50,000 customers
- Limited to US customers only
- Missing data for 15% of income field

## Known Limitations
- Data is US-centric and may not generalize
- Income is self-reported and may be inaccurate
- Low-income customers may be underrepresented

## Potential Biases
- Selection bias: Only includes customers who completed profile
- Survivorship bias: Excludes churned customers before 2020
```

### 5. **Beneficence**: Do No Harm

**Consider potential harms:**
- Could this reinforce stereotypes?
- Could this disadvantage vulnerable groups?
- Could this be misused?
- Are benefits distributed fairly?

**Example - Predictive policing:**
- ⚠️ May reinforce discriminatory policing patterns
- ⚠️ Historical data reflects biased enforcement
- ⚠️ Could create feedback loops
- ✅ Could improve resource allocation if done carefully
- ✅ Transparency about limitations is critical

---

## Specific Ethical Scenarios

### Scenario 1: Using Social Media Data

**Ethical considerations:**
```python
# ✅ Good practice
def ethical_twitter_analysis():
    """Analyze public tweets about climate change"""
    # 1. Only public tweets
    # 2. Aggregate analysis, no individual identification
    # 3. Don't republish tweet content verbatim
    # 4. Respect API terms of service
    # 5. Consider power dynamics (who has voice on Twitter?)
    
    tweets = collect_public_tweets(query="climate change")
    
    # Aggregate analysis
    sentiment_by_region = tweets.groupby('region')['sentiment'].mean()
    
    # Remove identifying information before saving
    tweets_anon = tweets.drop(['user_id', 'username', 'tweet_id'], axis=1)
    
    return sentiment_by_region
```

### Scenario 2: Health Data Research

**Special considerations:**
- **HIPAA** (US) and similar laws apply
- Requires IRB approval for human subjects research
- Extra protections for sensitive information
- Secure storage required

```python
# Security measures
def secure_health_data_analysis():
    """Handle sensitive health data properly"""
    # 1. Encrypt data at rest
    # 2. Use secure connections
    # 3. Log all access
    # 4. Minimize data retention
    # 5. De-identify before analysis
    
    # Example: k-anonymity
    from anonymizedf import anonymize
    
    anon_data = anonymize(
        health_data,
        k=5,  # At least 5 people per group
        quasi_identifiers=['age', 'zip', 'gender']
    )
    
    return anon_data
```

### Scenario 3: Predictive Models for Decision-Making

**When models affect people's lives:**

```python
def responsible_model_deployment():
    """Deploy models responsibly"""
    
    # 1. Test for fairness
    from aif360.metrics import BinaryLabelDatasetMetric
    
    metric = BinaryLabelDatasetMetric(
        dataset, 
        privileged_groups=[{'gender': 1}],
        unprivileged_groups=[{'gender': 0}]
    )
    
    print(f"Disparate impact: {metric.disparate_impact()}")
    # Should be close to 1.0 (equal treatment)
    
    # 2. Provide explanations
    from shap import TreeExplainer
    
    explainer = TreeExplainer(model)
    shap_values = explainer.shap_values(X)
    
    # Show why prediction was made
    
    # 3. Allow for human review
    # Flag edge cases for manual review
    confidence = model.predict_proba(X)
    uncertain = confidence.max(axis=1) < 0.7
    
    # 4. Monitor for drift
    # Regularly check if model is still fair and accurate
    
    # 5. Have an appeals process
    # Let people contest automated decisions
```

---

## IRB and Research Approval

Many research projects require Institutional Review Board (IRB) approval.

**When you need IRB approval:**
- Research involving human subjects
- Collecting data from people
- Using identifiable health information
- Studying vulnerable populations

**The IRB process:**
1. Submit research protocol
2. Explain data collection methods
3. Describe risks and benefits
4. Show informed consent process
5. Wait for approval before starting

**Tips for IRB applications:**
- Start early (can take months)
- Be thorough in documentation
- Explain how you'll protect privacy
- Describe data storage and destruction plans
- Show you've considered risks

---

## Data Sharing Ethics

**When sharing research data:**

```python
def prepare_data_for_sharing():
    """Prepare data for public sharing"""
    
    # 1. Remove identifiers
    data_clean = data.drop([
        'name', 'address', 'phone', 'email',
        'ssn', 'medical_record_number'
    ], axis=1)
    
    # 2. Generalize sensitive fields
    data_clean['income_bracket'] = pd.cut(
        data['income'],
        bins=[0, 30000, 60000, 100000, np.inf],
        labels=['Low', 'Medium', 'High', 'Very High']
    )
    
    # 3. Add noise to continuous variables
    data_clean['age'] += np.random.randint(-2, 3, len(data_clean))
    
    # 4. Check for re-identification risk
    # Ensure no unique combinations
    unique_combinations = data_clean.groupby([
        'age_bracket', 'gender', 'zip_prefix'
    ]).size()
    
    assert (unique_combinations >= 5).all(), "Re-identification risk!"
    
    # 5. Document what you did
    create_data_dictionary()
    
    return data_clean
```

**Include:**
- Clear documentation
- Data dictionary
- Limitations and known biases
- Appropriate license (e.g., CC-BY)
- Citation information

---

## Algorithmic Fairness

**Fairness definitions** (pick based on context):

1. **Demographic parity**: Equal positive rates across groups
2. **Equal opportunity**: Equal true positive rates across groups  
3. **Predictive parity**: Equal precision across groups

```r
library(fairness)

# Check multiple fairness metrics
fairness_check <- equal_odds(
  data = predictions,
  outcome = 'actual',
  prediction = 'predicted',
  group = 'protected_group'
)

print(fairness_check)

# Visualize disparities
ggplot(fairness_check) +
  geom_bar(aes(x = group, y = metric_value, fill = group)) +
  facet_wrap(~metric) +
  labs(title = "Fairness Metrics Across Groups")
```

---

## Your Ethics Checklist

Before starting research:

- ✅ IRB approval obtained (if needed)
- ✅ Data collection is ethical and legal
- ✅ Informed consent obtained (if needed)
- ✅ Privacy protections in place
- ✅ Potential biases identified
- ✅ Fairness metrics considered
- ✅ Limitations documented
- ✅ Benefits outweigh risks
- ✅ Vulnerable populations protected
- ✅ Plan for responsible sharing

---

## Resources

- **ACM Code of Ethics**: https://www.acm.org/code-of-ethics
- **Fairness in ML**: https://fairmlbook.org
- **AI Ethics Guidelines**: https://ai.google/principles
- **Data Science Ethics Course**: https://ethics.fast.ai
- **Your institution's IRB**: Contact them early!

---

## Key Takeaways

1. **Privacy matters**: Protect people's information
2. **Recognize bias**: In data, models, and yourself
3. **Be transparent**: Document everything
4. **Do no harm**: Consider negative impacts
5. **Get approval**: Follow proper procedures
6. **Test fairness**: Check for discriminatory outcomes
7. **Think critically**: Question your assumptions
8. **Stay informed**: Ethics evolves with technology

**Remember:** Good ethics is good science. Take the time to do it right!

---

*For more guidance, check out:*
- **[Research Ethics in Data Science](../research-ethics)** 
- **[Writing Your First Paper](../writing-papers)**
- **[Reproducible Research Code](../reproducible-code)**

*Do research that makes the world better!* 🌍✨
