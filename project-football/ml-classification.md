---
layout: default
title: Classification & Ensemble
---

# Classification & Ensemble ⚽🤖

After completing feature engineering and feature selection, the final stage was building machine learning classifiers to assign players into **general categories** (Defender, Midfielder, Forwarder) and then into their **specific field positions**.  

Because football players can often play multiple positions, this was treated as a **multi-label classification problem**.

---

## 🔹 Step 1: Removing Goalkeepers
Goalkeepers were excluded early in the process because their skill set (e.g., `goalkeeping_diving`, `goalkeeping_reflexes`) is fundamentally different from outfield players.  
This allowed us to focus on classifying only **Defenders, Midfielders, and Forwards**.

---

## 🔹 Step 2: General Category Classification
The first task was to assign each player into one (or more) of the broad categories:  
- **Defender**  
- **Midfielder**  
- **Forwarder**

Several models were tested, including **binary relevance**, **logistic regression**, and **neural networks**.  
👉 The **neural network** produced the best performance:

| Class       | Precision | Recall | F1-score | Support |
|-------------|-----------|--------|----------|---------|
| Defender    | 0.92      | 0.95   | 0.94     | 1809    |
| Midfielder  | 0.86      | 0.81   | 0.83     | 1703    |
| Forwarder   | 0.82      | 0.73   | 0.78     | 1160    |

- **Micro Avg F1**: 0.86  
- **Macro Avg F1**: 0.85  
- **Accuracy**: 0.668  

---

## 🔹 Step 3: Position-Level Classification
Once the general role was assigned, players were further classified into **specific field positions within their category**.  

### ⚔️ Defenders (Neural Network)
| Position | Precision | Recall | F1-score | Support |
|----------|-----------|--------|----------|---------|
| CB       | 0.89      | 0.85   | 0.87     | 814     |
| RB       | 0.83      | 0.74   | 0.78     | 423     |
| LB       | 0.86      | 0.72   | 0.78     | 415     |
| CDM      | 0.87      | 0.74   | 0.80     | 617     |

- **Micro Avg F1**: 0.82  
- **Accuracy**: 0.691  

---

### 🎯 Midfielders (Classifier Chains)
| Position | Precision | Recall | F1-score | Support |
|----------|-----------|--------|----------|---------|
| CM       | 0.87      | 0.87   | 0.87     | 831     |
| RM       | 0.63      | 0.62   | 0.62     | 499     |
| LM       | 0.63      | 0.55   | 0.59     | 474     |
| CAM      | 0.63      | 0.40   | 0.49     | 459     |

- **Micro Avg F1**: 0.69  
- **Accuracy**: 0.511  

---

### ⚡ Forwards (Neural Network)
| Position | Precision | Recall | F1-score | Support |
|----------|-----------|--------|----------|---------|
| ST       | 0.88      | 0.89   | 0.88     | 650     |
| LW       | 0.73      | 0.39   | 0.51     | 348     |
| RW       | 0.82      | 0.27   | 0.41     | 312     |
| CF       | 0.00      | 0.00   | 0.00     | 95      |
| LWB      | 0.88      | 0.78   | 0.83     | 123     |
| RWB      | 0.94      | 0.71   | 0.81     | 82      |

- **Micro Avg F1**: 0.70  
- **Accuracy**: 0.545  

---

## 🔹 Step 4: Ensemble of Models
Finally, the outputs from all classifiers were combined into an **ensemble pipeline**.  
To better reflect real-world flexibility, we used **Relaxed Accuracy**, meaning the model is considered correct if **at least one predicted position** matches the player’s actual role.

- **Relaxed Accuracy**: **0.7522** 🎉  

This is a strong performance given the complexity of a **20+ multi-label classification task**.

---

## ✅ Key Takeaways
- Goalkeepers excluded to simplify the model.  
- Best model for general categories: **Neural Network**.  
- Best models for position-level:  
  - Defenders → Neural Network  
  - Midfielders → Classifier Chains  
  - Forwards → Neural Network  
- Ensemble pipeline achieved **~75% relaxed accuracy** across all positions.  

This approach demonstrates how machine learning can effectively classify football players based on skill metrics, with potential business applications in **scouting, transfer market analysis, and player development**.
