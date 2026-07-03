# Rain Tomorrow Prediction with Missing Value Handling and Feature Engineering

## Project Overview
This project aims to predict whether it will rain tomorrow using historical weather observations.  
The workflow goes beyond building a simple classification model, as it focuses on applying a complete preprocessing pipeline before training the model.

A major part of the project was dedicated to handling **missing values**, transforming categorical data into numerical form, extracting useful information from the date column, and preparing the data properly for machine learning.  
The final model was built using **Logistic Regression** after completing all preprocessing steps.

---

## Project Objective
The main objective of this project is to build a machine learning model that predicts the **RainTomorrow** target variable based on weather-related features such as temperature, humidity, rainfall, pressure, wind direction, and sunshine.

---

## Dataset Description
The dataset contains daily weather records and includes both **categorical** and **numerical** features.

### Examples of categorical features:
- `Location`
- `WindGustDir`
- `WindDir9am`
- `WindDir3pm`
- `RainToday`
- `RainTomorrow`

### Examples of numerical features:
- `MinTemp`
- `MaxTemp`
- `Rainfall`
- `Evaporation`
- `Sunshine`
- `WindSpeed9am`
- `Humidity3pm`
- `Pressure9am`

The target column used in this project is:

- **RainTomorrow**
  - `1` → Rain
  - `0` → No Rain

---

# Project Workflow

## 1. Data Exploration
The dataset was explored first to understand:
- the structure of the data
- feature types
- the target variable
- the existence of missing values
- the general quality of the dataset

This step helped identify the preprocessing tasks required before training the model.

---

## 2. Missing Value Analysis
A missing value analysis was performed to detect which columns contained missing data and to measure the impact of missing values on the dataset.

A visualization was created to show the columns with the highest number of missing values.  
This helped distinguish between columns with minor missing values and columns that required more attention during preprocessing.

---

## 3. Handling Missing Values in Categorical Columns
Missing values in categorical columns were handled using the **most frequent value (mode)** of each feature.

This strategy was selected because the proportion of missing values in the categorical columns was relatively small compared to the full dataset, so replacing missing values with the most common category was a practical and reliable solution.

This approach helped preserve the existing distribution of the categorical data without removing rows and losing information.

---

## 4. Handling Missing Values in Numerical Columns
Missing values in numerical columns were handled using **SimpleImputer** with the **median** strategy.

The median was chosen instead of the mean because weather-related numerical features may contain:
- skewed distributions
- extreme observations
- natural outliers

Using the median makes the imputation process more robust because it is less affected by unusual values than the mean.

---

## 5. Feature Engineering from the Date Column
The `Date` column was converted into datetime format, and three new features were extracted from it:

- `year`
- `month`
- `day`

After extracting these new time-based features, the original `Date` column was removed.

This step allows the model to capture possible seasonal and temporal patterns that may affect rainfall prediction.

---

## 6. Encoding Categorical Variables
Categorical variables were transformed into numerical format using two different encoding techniques depending on the type of feature.

### Label Encoding
**Label Encoding** was used for binary categorical columns such as:
- `RainToday`
- `RainTomorrow`

### One-Hot Encoding
**One-Hot Encoding** was used for categorical columns with multiple categories such as:
- `Location`
- `WindGustDir`
- `WindDir9am`
- `WindDir3pm`

This step was necessary because machine learning models cannot work directly with raw text categories.

---

## 7. Feature Scaling
The dataset contains numerical features measured on very different scales, such as temperature, rainfall, humidity, wind speed, and pressure.  
To make these features more comparable, **StandardScaler** was applied before training the model.

Feature scaling is especially important for models such as **Logistic Regression**, because it improves optimization during training and helps the model converge more effectively.

---

## 8. Train-Test Split
The dataset was divided into training and testing sets using `train_test_split()`.

A **stratified split** was used to preserve the original distribution of the target classes in both the training and testing data.  
This is important in classification problems because it prevents one class from being overrepresented in one split and underrepresented in the other.

---

## 9. Model Building
After completing all preprocessing steps, a **Logistic Regression** model was trained to predict whether it will rain tomorrow.

Logistic Regression was selected as a baseline classification model because it is simple, interpretable, and performs well when the data is properly prepared.

---

## 10. Model Evaluation
The model was evaluated using:
- **Training Accuracy**
- **Testing Accuracy**
- **Confusion Matrix**
- **Classification Report**

### Logistic Regression Performance
- **Training Accuracy:** approximately **0.85**
- **Testing Accuracy:** approximately **0.85**

The close values between training and testing accuracy indicate that the model generalizes reasonably well and does not show strong signs of overfitting.

---

# Confusion Matrix Interpretation

The confusion matrix provides a detailed view of the model’s classification performance:

- **TN = 21552** → The model correctly predicted **no rain**.
- **FP = 1165** → The model predicted **rain**, but it actually did **not rain**.
- **FN = 3171** → The model predicted **no rain**, but it actually **rained**.
- **TP = 3204** → The model correctly predicted **rain**.

### Interpretation
The model performs very well in identifying **days without rain**, as shown by the high number of true negatives.  
It is also able to detect a reasonable number of rainy days, but it still misses some actual rain cases, as reflected in the false negatives.

Overall, the model shows **good classification performance**, with stronger accuracy in predicting **no-rain days** than rainy days.
