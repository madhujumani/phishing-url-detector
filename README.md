# 🎣 Phishing URL Detector

![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-green?logo=scikit-learn&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-lightgrey)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

> A beginner-friendly machine learning project built for a **Data Analytics with AI** internship.  
> Detects phishing URLs using hand-crafted features — no page loading, no network calls, instant results.

---

## 🔍 What it does

Given any URL, the system:
- Extracts **9 numerical features** from the URL string
- Runs it through a trained **Random Forest** or **Logistic Regression** model
- Returns a **verdict** (Phishing / Legitimate), a **confidence %**, and a **risk score 0–100**

```
URL        : http://192.168.1.1/paypal-login-verify-account.php
Risk Score :  100.0 / 100  [████████████████████]
Risk Level : HIGH RISK 🚨

URL        : https://www.google.com
Risk Score :    0.0 / 100  [░░░░░░░░░░░░░░░░░░░░]
Risk Level : SAFE ✅
```

---

## 📦 Dataset

| Property | Detail |
|---|---|
| Reference | [PhiUSIIL Phishing URL Dataset — UCI ML Repository](https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset) |
| Built-in sample | 2,000 URLs (1,000 legitimate + 1,000 phishing) — no download needed |
| Label | `1` = Phishing · `0` = Legitimate |
| Balance | 50 / 50 — perfectly balanced |

---

## 🗂 Project Structure

```
phishing-url-detector/
├── phishing_url_detector.ipynb   ← Main Jupyter notebook (11 sections)
├── phishing_data.csv             ← Generated dataset
├── requirements.txt              ← Python dependencies
├── report.md                     ← Full written project report
├── presentation_notes.md         ← Slide-by-slide talking points
├── LICENSE                       ← MIT License
└── README.md                     ← This file
```

---

## 📒 Notebook Sections

| # | Section | Highlights |
|---|---|---|
| 0 | **Imports** | All libraries in one cell |
| 1 | **Data Loading & Cleaning** | Built-in dataset, null checks, type validation |
| 2 | **EDA** | 4 charts — class balance, URL length, dot count, summary stats |
| 3 | **Feature Extraction** | 9 features extracted from raw URL strings |
| 4 | **Model Training** | Logistic Regression + Random Forest (200 trees) |
| 5 | **Model Comparison** | Accuracy, Precision, Recall, F1, Confusion Matrices |
| 6 | **URL Checker** | `predict_url(url)` — instant verdict + confidence |
| 7 | **Shannon Entropy** | Unique bonus feature — measures URL randomness |
| 8 | **ROC Curve** | AUC comparison — industry-standard security metric |
| 9 | **Risk Score** | 0–100 score with visual bar indicator |
| 10 | **Batch Scanner** | `scan_batch(urls)` — scan many URLs at once, ranked by risk |
| 11 | **Summary** | Results recap + ideas to extend the project |

---

## 🔬 Features Extracted from URLs

| Feature | Description | Phishing Signal |
|---|---|---|
| `url_length` | Total character count | Longer = more suspicious |
| `num_dots` | Number of `.` characters | More dots = more subdomains |
| `num_hyphens` | Number of `-` characters | Used to mimic brands |
| `has_at` | `@` symbol present | Tricks browser into ignoring real domain |
| `has_ip` | Raw IPv4 address used | Legitimate sites use domain names |
| `is_https` | Starts with `https://` | HTTP = no encryption |
| `num_subdomains` | Subdomain depth | Abuse of free subdomain services |
| `has_suspicious_word` | Keywords like `login`, `verify`, `secure` | Phishers overuse these |
| `entropy` | Shannon entropy of URL string | High randomness = generated URL |

---

## 📊 Results

| Model | Accuracy | Precision | Recall | F1 | AUC |
|---|---|---|---|---|---|
| Logistic Regression | ~0.99 | ~0.99 | ~0.99 | ~0.99 | ~0.99 |
| Random Forest | ~1.00 | ~1.00 | ~1.00 | ~1.00 | ~1.00 |

> Both models achieve near-perfect scores. Random Forest edges ahead on all metrics.  
> **Recall** is the most critical metric for security — it measures how many phishing URLs were actually caught.

---

## 🚀 Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/YOUR_USERNAME/phishing-url-detector.git
cd phishing-url-detector
```

### 2. Create a virtual environment
```bash
# Windows
python -m venv .venv
.venv\Scripts\python.exe -m pip install -r requirements.txt

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### 3. Launch Jupyter
```bash
# Windows
.venv\Scripts\python.exe -m jupyter notebook

# macOS / Linux
jupyter notebook
```

Open `phishing_url_detector.ipynb` → **Kernel → Restart & Run All**

---

## 🧪 Try it yourself

In the last interactive cell of the notebook:

```python
# Change this URL to anything you want to test
my_url = 'https://www.google.com'

predict_url(my_url, model='rf')   # Random Forest
predict_url(my_url, model='lr')   # Logistic Regression
```

Or use the batch scanner:
```python
scan_batch([
    'https://www.google.com',
    'http://192.168.1.1/paypal-login-verify-account.php',
    'http://secure-banking-login.verify-account.xyz/update',
])
```

---

## 🛠 Tech Stack

| Library | Version | Purpose |
|---|---|---|
| `pandas` | latest | Data loading and manipulation |
| `numpy` | latest | Numerical operations |
| `scikit-learn` | latest | ML models and evaluation |
| `matplotlib` | latest | Charts and visualisations |
| `seaborn` | latest | Statistical chart styling |
| `notebook` | latest | Jupyter notebook server |

---

## 💡 Ideas to Extend

| Idea | Difficulty |
|---|---|
| Add `num_slashes`, `path_depth` features | ⭐ Easy |
| Use `tldextract` for clean domain/TLD features | ⭐ Easy |
| Tune Random Forest with `GridSearchCV` | ⭐⭐ Medium |
| Try XGBoost or Gradient Boosting | ⭐⭐ Medium |
| Add WHOIS domain age lookup | ⭐⭐⭐ Hard |
| Deploy as a FastAPI web service | ⭐⭐⭐ Hard |
| Build a Chrome browser extension | ⭐⭐⭐ Hard |

---

## 📄 License

This project is released under the [MIT License](LICENSE).  
The PhiUSIIL dataset is subject to [UCI ML Repository terms](https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset).

---

## 👤 Author

Built as part of a **Data Analytics with AI** internship project.  
Feel free to fork, star ⭐, and build on top of it!
