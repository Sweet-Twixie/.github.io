---
layout: default
title: Feature Selection
---

# Feature Selection ⚡

Feature selection was a key step in the project to identify the most predictive skills for classifying football players into general categories: **Defender, Midfielder, Forwarder**. This ensures the model focuses on the features that matter most, improving performance and interpretability.

## 🔹 Initial Feature Set
We started with a comprehensive set of engineered features representing player skills, physical attributes, and mentality metrics, including:

- Attacking: crossing, finishing, heading accuracy, short passing, volleys  
- Skill: dribbling, curve, long passing, ball control  
- Movement: acceleration, sprint speed, agility, balance  
- Power: shot power, jumping, stamina, strength  
- Mentality: vision, interceptions, positioning  
- Pace and shooting metrics (standardized or transformed)  

A total of **40+ features** were included in the cleaned dataset.

## 🔹 Target Categories
The model predicts **general player roles**:

- Defender  
- Midfielder  
- Forwarder  

### Subcategories (used in further modeling)
- CB, RB, LB, CDM, CM, LM, RM, CAM, ST, LW, RW, CF, RWB, LWB  

---

## 🔹 Feature Selection Methods
We applied three complementary approaches to determine which features were most important for classification:

1. **Random Forest Feature Importance**  
   - Trained a Random Forest classifier on the dataset.  
   - Ranked features by importance and selected the top 10.  

2. **Recursive Feature Elimination (RFE)**  
   - Used RFE with a Random Forest estimator to iteratively select the most relevant features.  
   - Chose 10 features that consistently contributed to accurate predictions.  

3. **Gradient Boosting Feature Importance**  
   - Trained a Gradient Boosting classifier.  
   - Selected the top 10 features by importance score.  

**Selected features from each method:**
```text
Random Forest Selected Features: [list rf_selected_features here]
RFE Selected Features: [list rfe_selected_features here]
Gradient Boosting Selected Features: [list gb_selected_features here]

## 🔹 Feature Selection Code Example

Below is the Python code used to select features for the main player categories:

```python
from sklearn.ensemble import RandomForestClassifier, GradientBoostingClassifier
from sklearn.feature_selection import RFE
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
import pandas as pd

X = df_sub_categories_no_GK.drop(['Defender', 'Midfielder', 'Forwarder'], axis = 1)
y = df_sub_categories_no_GK[['Defender', 'Midfielder', 'Forwarder']]

y_encoded = LabelEncoder().fit_transform(y.values.argmax(axis=1))

X_train, X_test, y_train, y_test = train_test_split(X, y_encoded, test_size=0.2, random_state=42)

# 1. Random Forest for Feature Importance
rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)
rf_feature_importance = pd.Series(rf_model.feature_importances_, index=X_train.columns)
rf_selected_features = rf_feature_importance.sort_values(ascending=False).head(10)

# 2. Recursive Feature Elimination (RFE)
rfe_model = RFE(estimator=RandomForestClassifier(n_estimators=100, random_state=42), n_features_to_select=10)
rfe_model.fit(X_train, y_train)
rfe_selected_features = X_train.columns[rfe_model.support_]

# 3. Gradient Boosting for Feature Importance
gb_model = GradientBoostingClassifier(n_estimators=100, random_state=42)
gb_model.fit(X_train, y_train)
gb_feature_importance = pd.Series(gb_model.feature_importances_, index=X_train.columns)
gb_selected_features = gb_feature_importance.sort_values(ascending=False).head(10)

print("Random Forest Selected Features:\n", rf_selected_features)
print("RFE Selected Features:\n", rfe_selected_features)
print("Gradient Boosting Selected Features:\n", gb_selected_features)
