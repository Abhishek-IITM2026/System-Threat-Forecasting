# 🛡️ System Threat Forecaster

Predicting Malware Infections Using Machine Learning and Advanced Feature Engineering

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML-orange)
![Kaggle](https://img.shields.io/badge/Kaggle-Competition-20BEFF)
![Accuracy](https://img.shields.io/badge/Accuracy-0.6378-success)

## 📌 Project Overview

Modern computer systems generate large amounts of telemetry data through antivirus software and operating system monitoring tools. These signals contain valuable information about system security, configuration, hardware characteristics, and software behavior.

The objective of this project is to predict whether a system is likely to become infected by malware based on its telemetry data. The project was developed as part of the **Machine Learning Practice (MLP) Project T12025** and submitted to the **System Threat Forecaster Kaggle Competition**.

The challenge is formulated as a **binary classification problem**, where:

- **0** → No malware detected
- **1** → Malware detected

The final solution combines extensive exploratory data analysis, feature engineering, preprocessing pipelines, hyperparameter optimization, and ensemble learning techniques.

---

## 🎯 Problem Statement

Can we forecast whether a machine will be infected by malware before the infection actually occurs?

Using system telemetry collected from antivirus threat reports, the model learns patterns associated with compromised systems and predicts the probability of future infections.

---

## 📊 Dataset Description

Each row in the dataset represents a unique machine identified by a `MachineID`.

### Dataset Files

- `train.csv` – Training dataset containing features and target labels
- `test.csv` – Test dataset for prediction
- `sample_submission.csv` – Submission template

### Target Variable

| Value | Meaning |
|---------|---------|
| 0 | No Malware Detected |
| 1 | Malware Detected |

### Dataset Characteristics

The dataset contains:

- Antivirus information
- Operating system details
- Hardware specifications
- Security configurations
- Firmware information
- Regional information
- Device characteristics
- Update history

Total features: **75+ system telemetry features**

---

# 🔍 Exploratory Data Analysis (EDA)

Several exploratory analyses were performed to understand the dataset and identify useful patterns.

## Class Distribution Analysis

The target variable was found to be nearly balanced:

- Class 0 (No Threat): ~49.5%
- Class 1 (Threat Detected): ~50.5%

This reduced the need for aggressive resampling techniques.

## Correlation Analysis

A correlation heatmap was generated to identify:

- Redundant features
- Strongly correlated variables
- Potential feature combinations

## Scatter Plot Analysis

Relationships explored include:

- Display resolution dimensions
- Disk capacities
- Language settings
- Processor specifications

## Missing Value Analysis

Missing values were analyzed for:

- Numerical features
- Categorical features
- Date-related columns

Instead of dropping columns, appropriate imputation techniques were applied.

## Skewness Analysis

Feature skewness was computed to identify:

- Highly skewed numerical features
- Features requiring transformation

---

# ⚙️ Feature Engineering

Feature engineering played a major role in improving predictive performance.

## 1. OSGenuineState Transformation

Converted:

```text
IS_GENUINE → 1
Others → 0
```

This creates a cleaner binary security indicator.

---

## 2. Date Processing

The following columns were converted into datetime format:

```text
DateAS
DateOS
```

Missing dates were imputed and additional temporal information was extracted.

---

## 3. Correlation-Based Feature Reduction

Highly correlated features were:

- Combined when beneficial
- Removed when redundant

This reduced multicollinearity and model complexity.

---

## 4. Binary Feature Encoding

Binary categorical variables were transformed into:

```text
Yes / No
True / False
0 / 1
```

representations suitable for machine learning models.

---

# 🧹 Data Preprocessing Pipeline

A complete preprocessing pipeline was implemented using:

```python
Pipeline
ColumnTransformer
```

from Scikit-Learn.

---

## Missing Value Handling

### Numerical Features

```python
SimpleImputer(strategy="median")
```

### Categorical Features

```python
SimpleImputer(strategy="most_frequent")
```

---

## Feature Transformation

### Quantile Transformer

Used for highly skewed features.

Benefits:

- Makes distributions more Gaussian
- Reduces influence of outliers
- Improves model performance

```python
QuantileTransformer(output_distribution="normal")
```

---

## Scaling

### RobustScaler

Applied to features containing significant outliers.

```python
RobustScaler()
```

### StandardScaler

Applied to relatively clean numerical features.

```python
StandardScaler()
```

---

# 🤖 Machine Learning Models

Several machine learning algorithms were trained and evaluated.

## 1. XGBoost Classifier

Hyperparameter tuning performed using:

```python
RandomizedSearchCV
```

### Tuned Parameters

- max_depth
- learning_rate
- n_estimators
- min_child_weight
- colsample_bytree
- subsample
- reg_alpha
- reg_lambda

---

## 2. Random Forest Classifier

Configuration:

```python
n_estimators = 300
max_depth = 10
```

Used as both:

- Standalone classifier
- Stacking ensemble component

---

## 3. LightGBM Classifier

Configuration:

```python
n_estimators = 300
max_depth = 8
learning_rate = 0.05
```

Chosen for:

- Fast training
- High predictive power
- Strong handling of tabular data

---

## 4. Extra Trees Classifier

Configuration:

```python
n_estimators = 300
max_depth = 15
```

Provides additional diversity to ensemble learning.

---

# 🏗️ Ensemble Learning

## Stacking Classifier

A stacking ensemble was built using:

### Base Models

- XGBoost
- Random Forest
- LightGBM
- Extra Trees

### Meta Learner

```python
RandomForestClassifier
```

The stacking architecture combines predictions from multiple models to improve generalization.

---

# 📈 Evaluation Metrics

Models were evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC Score

---

## Best Results

| Metric | Best Model | Score |
|----------|------------|---------|
| Accuracy | XGBoost | 0.6312 |
| Precision | XGBoost | 0.6271 |
| F1 Score | XGBoost | 0.6462 |
| Recall | LightGBM | 0.6684 |
| ROC-AUC | LightGBM | 0.6828 |

---

# 🏆 Final Conclusion

Key findings from the project:

- XGBoost achieved the highest classification performance.
- LightGBM produced the strongest ROC-AUC score.
- Feature engineering significantly improved model effectiveness.
- Quantile transformation successfully reduced skewness.
- Ensemble stacking provided robustness but slightly underperformed compared to the best standalone gradient boosting models.

Overall, **XGBoost and LightGBM emerged as the most effective models for malware threat prediction**.

---

# 🛠️ Technologies Used

## Programming Language

- Python

## Data Analysis

- Pandas
- NumPy

## Visualization

- Matplotlib
- Seaborn

## Machine Learning

- Scikit-Learn
- XGBoost
- LightGBM

## Statistical Analysis

- SciPy

## Model Optimization

- RandomizedSearchCV
- Stratified K-Fold Cross Validation

---

# 📂 Project Structure

```text
System-Threat-Forecaster/
│
├── train.csv
├── test.csv
├── sample_submission.csv
│
├── notebook.ipynb
│
├── submission.csv
│
├── README.md
│
└── models/
    └── trained_model.pkl
```

---

# 🚀 Future Improvements

- Advanced feature selection techniques
- AutoML experimentation
- Deep learning approaches
- CatBoost implementation
- Explainable AI (SHAP & LIME)
- Malware risk probability estimation
- Model deployment using Flask/FastAPI

---

# 👨‍💻 Author

**Abhishek Dutta**

BS in Data Science and Programming
Indian Institute of Technology Madras (IIT Madras)

LinkedIn: *Add Your LinkedIn Profile*  
Kaggle: *Add Your Kaggle Profile*

---

## Kaggle Competition

System Threat Forecaster – Malware Infection Prediction Challenge

Evaluation Metric:

```python
accuracy_score()
```

Goal:

Predict whether a machine is likely to be infected by malware using telemetry data generated from antivirus threat reports.
