# 🏏 IPL Win Probability Predictor

A machine learning web application that predicts the **real-time win probability** of an IPL T20 match during the second innings chase — built with Python, Scikit-learn, and Streamlit.

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=for-the-badge&logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-ML-orange?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-App-red?style=for-the-badge&logo=streamlit&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data-green?style=for-the-badge&logo=pandas&logoColor=white)

---

## 📌 Overview

Given a live match situation — batting team, bowling team, venue, target, current score, overs bowled, and wickets fallen — the model outputs the **win and loss probability** for both teams in real time.

The model was trained on **756 IPL matches** (2008–2019) using ball-by-ball delivery data, and achieves **~80.3% accuracy** on the test set.

---

## 🎯 Features

- Real-time win probability prediction for the **chasing team**
- Supports all **8 active IPL franchises**
- Covers **29 IPL venues/cities**
- Clean, minimal **Streamlit web interface**
- Trained on a **Logistic Regression pipeline** with One-Hot Encoding

---

## 🖥️ Demo

Enter the current match situation:

| Input | Example |
|---|---|
| Batting Team | Mumbai Indians |
| Bowling Team | Chennai Super Kings |
| Host City | Mumbai |
| Target | 180 |
| Current Score | 90 |
| Overs Completed | 10 |
| Wickets Out | 3 |

**Output:**
```
Mumbai Indians  →  62%
Chennai Super Kings  →  38%
```

---

## 🗂️ Project Structure

```
ipl-win-probability-predictor/
│
├── app.py                  # Streamlit web application
├── Untitled.ipynb          # Data preprocessing + model training notebook
├── pipe.pkl                # Trained ML pipeline (serialized)
├── matches.csv             # Match-level dataset (756 matches)
├── deliveries.csv          # Ball-by-ball dataset (179,078 deliveries)
├── requirements.txt        # Python dependencies
├── Procfile                # Heroku deployment config
├── setup.sh                # Streamlit server config for deployment
└── README.md
```

---

## 🧠 How It Works

### 1. Data Preprocessing
- Loaded `matches.csv` and `deliveries.csv`
- Computed **1st innings total** for each match (the target)
- Merged match-level info into ball-by-ball delivery data
- Filtered to only **2nd innings** deliveries (chasing phase)
- Removed **Duckworth-Lewis affected** matches
- Standardized team names (e.g., *Delhi Daredevils → Delhi Capitals*)
- Kept only the **8 currently active IPL teams**

### 2. Feature Engineering
For every ball in the 2nd innings, the following features were computed:

| Feature | Description |
|---|---|
| `batting_team` | Team currently chasing |
| `bowling_team` | Team defending the target |
| `city` | Host city of the match |
| `runs_left` | Runs still needed to win |
| `balls_left` | Balls remaining in the innings |
| `wickets` | Wickets still in hand (10 - fallen) |
| `total_runs_x` | 1st innings target score |
| `crr` | Current Run Rate of chasing team |
| `rrr` | Required Run Rate to win |

### 3. Model Training
- Text columns (`batting_team`, `bowling_team`, `city`) encoded using **OneHotEncoder** (drop='first')
- Numeric columns passed through unchanged via `ColumnTransformer`
- **Logistic Regression** (solver=`liblinear`) trained on 80% of data
- Pipeline serialized using `pickle` → `pipe.pkl`

### 4. Prediction
- `pipe.predict_proba()` returns `[loss_probability, win_probability]`
- Displayed as percentages in the Streamlit app

---

## 📊 Model Performance

| Metric | Value |
|---|---|
| Algorithm | Logistic Regression |
| Train/Test Split | 80% / 20% |
| Accuracy | **80.3%** |
| Features | 9 (3 categorical + 6 numeric) |
| Training Samples | ~65,000+ ball snapshots |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.8 or above
- pip

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/ipl-win-probability-predictor.git
cd ipl-win-probability-predictor

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the Streamlit app
streamlit run app.py
```

The app will open at `http://localhost:8501` in your browser.

> **Note:** `pipe.pkl` must be present in the same directory as `app.py`. If it is missing, run `Untitled.ipynb` end-to-end to regenerate it.

---

## 📦 Dependencies

```
streamlit
pandas
numpy
scikit-learn
```

Install all at once:
```bash
pip install -r requirements.txt
```

---

## 📁 Dataset

| File | Rows | Description |
|---|---|---|
| `matches.csv` | 756 | One row per IPL match (2008–2019) |
| `deliveries.csv` | 179,078 | One row per ball bowled |

The datasets are sourced from public IPL ball-by-ball records available on Kaggle.

---

## 🏟️ Supported Teams

| Team |
|---|
| Chennai Super Kings |
| Delhi Capitals |
| Kolkata Knight Riders |
| Kings XI Punjab |
| Mumbai Indians |
| Rajasthan Royals |
| Royal Challengers Bangalore |
| Sunrisers Hyderabad |

---

## ⚠️ Limitations

- Only supports the **8 current IPL franchises** (no newer teams like Lucknow, Gujarat Titans)
- Dataset covers seasons **up to 2019** — does not include recent seasons
- Does not account for **player form, injuries, or weather conditions**
- Duckworth-Lewis matches are excluded from training data

---

## 🔮 Future Improvements

- [ ] Update dataset with IPL 2020–2024 seasons
- [ ] Add support for newer franchises (LSG, GT, SRH new branding)
- [ ] Experiment with Random Forest / XGBoost for higher accuracy
- [ ] Add over-by-over win probability chart in the app
- [ ] Deploy on Streamlit Cloud / Render

---


---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---
