
# 🧪 SmellDB

  

>  **The first ML benchmark for artificial olfaction — evaluations coming soon.**

  

[![Dataset (Training)](https://img.shields.io/badge/Dataset%20%28Training%29-Download-black)](https://smelldb.anemolabs.com/dataset/download) [![Dataset (Evaluation)](https://img.shields.io/badge/Dataset%20%28Evaluation%29-Coming%20Soon-f59e0b)](https://smelldb.anemolabs.com) [![Paper](https://img.shields.io/badge/Paper-arXiv-red)](https://arxiv.org) [![Leaderboard](https://img.shields.io/badge/Leaderboard-Coming%20Soon-f59e0b)](https://smelldb.anemolabs.com/leaderboard) 
   

## Overview

  

Smell is perhaps our most complicated and misunderstood sense. **SmellDB** is the first publicly available benchmark with a dataset that benchmarks AI models on olfactory data. 

 At the moment, entries are API-first. Download the dataset, build your model, and submit predictions via our REST API. Scores are returned instantly and performance is optionally logged on a public leaderboard.

 The held-out test data as well as the benchmark will be made available in the near future.

  

### Highlights
| | |
|---|---|
| 🗂 **Samples** | 3,000 labelled samples |
| ⏱ **Duration** | 115 seconds per sample |
| 📡 **Channels** | 32 sensor channels |
| 🕐 **Total recording** | 96 hours |
| 🏷 **Classes** | 12 semantic smell labels |

  
  

## The Task

  

Given multichannel time-series sensor data from a 32-channel electronic nose (e-Nose), predict the **1-of-12 smell label** for each sample.

  | # | Label |
|---|---|
| 1 | 🍊 Sweet Orange |
| 2 | 🪵 Leather & Tobacco |
| 3 | 🍂 Cinnamon Leaf |
| 4 | 🍌 Banana |
| 5 | ☕ Cafe Latte |
| 6 | 🌹 Rose |
| 7 | 💜 Lavender Oil |
| 8 | 🥥 Coconut |
| 9 | 🥭 Mango |
| 10 | 🌸 English Orchid |
| 11 | 🧴 Monkey Farts |
| 12 | 💧 Water |

 

  

## Getting Started

  ### 1. Get your API key

Register by making two API calls to `https://smelldb.anemolabs.com`.

**( a ) - Register.** A 6-digit verification code will be sent to your email.

```bash
curl -X POST https://smelldb.anemolabs.com/v1/user/register \
  -H "Content-Type: application/json" \
  -d '{"email": "you@example.com", "name": "Your Name", "organisation": "Optional"}'
```

**( b ) - Activate.** Your API key will be emailed to you on success.

```bash
curl -X POST https://smelldb.anemolabs.com/v1/user \
  -H "Content-Type: application/json" \
  -d '{"action": "activate_user", "email": "you@example.com", "verification_code": "123456"}'
```

**( c ) - Rotate API key (optional).** If you need to invalidate your current key, a new one will be emailed to you.

```bash
curl -X POST https://smelldb.anemolabs.com/v1/user \
-H "Content-Type: application/json" \`
-d '{"action": "request_new_api_key", "email": "you@example.com"}'
```
----------

### 2. Download the dataset

```bash
https://smelldb.anemolabs.com/dataset/download
```

```text
YOUR_DIRECTORY/
├── dataset.csv     # sensor readings (N samples × 32 channels) and labels (final 'LABEL' column)

```

----------

### 3. Build your model

```python
import pandas as pd

df = pd.read_csv("YOUR_DIRECTORY/dataset.csv")
y = df['LABEL']
X = df.drop('LABEL', axis=1)

# Transform Data

# Train model
model.fit(X,y)

# Generate smell label predictions here
predictions = model.predict(X)   # list of label strings

```

----------

### 4. Submit

```python
import requests

response = requests.post(
    "https://smelldb.anemolabs.com/v1/evals",
    headers={
        "X-API-Key": "YOUR_API_KEY",
        "Content-Type": "application/json",
    },
    json={
        "dataset_name": "smelldb-base-v1",
        "predictions": predictions.tolist(),
    },
)

print(response.json())

```
----------
###  5. View your submissions

```python
response = requests.get(
    "https://smelldb.anemolabs.com/v1/evals",
    headers={
        "X-API-Key": "YOUR_API_KEY",
    },
)
print(response.json())
``` 

  


