---
layout: default
title: Feature Engineering
---

# Feature Engineering ⚙️

Feature engineering was a critical step in transforming raw football data into meaningful inputs for machine learning models. The dataset contained **20,000+ players** with categorical, numerical, and text-based attributes. My goal was to reshape this data so the models could better capture player roles and performance.

---

## 🔹 Encoding Player Positions
Each player could play in multiple positions, stored as text like `"CF, ST, LM"`. To make this usable, I converted the `"player_positions"` field into structured features:

- **General categories**:  
  - **Defender** → CB, RB, LB, CDM  
  - **Midfielder** → CM, LM, RM, CAM  
  - **Forwarder** → ST, LW, RW, CF, LWB, RWB  
  - **Goalkeeper** → GK  

- **Specific positions**: A binary column for each role (e.g., CB=1, ST=0).  

👉 Goalkeepers were excluded from further modeling since they can be trivially identified from their unique goalkeeping stats.

This gave every player a clear **multi-label representation**:  
- General role (Defender/Midfielder/Forwarder).  
- All specific positions they can play.  

---

## 🔹 Handling Left/Right Roles
Some positions appear in left/right pairs (e.g., **LB/RB**, **LW/RW**). To reduce redundancy, I tested combining them into aggregated features:

- `LW/RW`  
- `LB/RB`  
- `LM/RM`  
- `LWB/RWB`  

Values could be:  
- `0` → player doesn’t play either role.  
- `1` → player plays one of them.  
- `2` → player can play both.  

However, since very few players had value `2`, I ultimately kept **left/right roles separate** to preserve detail.

---

## 🔹 Numerical Feature Engineering
Beyond positions, the dataset contained **40+ numerical attributes** describing skills (e.g., dribbling, crossing, stamina, vision). To make these features comparable and more predictive:

- **Standardization** → scaled attributes to have mean=0 and variance=1.  
- **Binning** → grouped continuous values (e.g., pace ratings) into categorical bins to reduce noise.  
- **Box-Cox transformation** → applied to skewed attributes to make their distributions more Gaussian, improving model performance.  

---

## ✅ Outcome
After feature engineering, the dataset was clean, structured, and model-ready:  
- **Categorical text fields** (positions) were transformed into interpretable binary/multi-label features.  
- **Numerical attributes** were standardized, normalized, or transformed for stability.  
- **Goalkeepers** were excluded from outfield classification, ensuring focus on the three main roles: **Defender, Midfielder, Forwarder**.  

This solid foundation allowed for meaningful **feature selection** and accurate **multi-label classification** in the later modeling stages.
