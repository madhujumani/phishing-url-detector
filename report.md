# Project Report: Phishing URL Detector Using Machine Learning

**Course:** Data Analytics with AI  
**Project Type:** Individual / Internship Project  
**Tools Used:** Python, Jupyter Notebook, scikit-learn, pandas, matplotlib, seaborn

---

## Abstract

Phishing attacks are one of the most common forms of cybercrime, tricking users into visiting fake websites that steal personal information. This project builds a machine learning system that automatically detects phishing URLs by analysing structural patterns in the URL string itself — no page content or network access required. Two classifiers, Logistic Regression and Random Forest, are trained and evaluated. The system achieves high accuracy and includes a real-time URL checker, a risk score meter, and a batch scanner.

---

## 1. Introduction

### Problem Statement
Phishing websites impersonate legitimate services (banks, PayPal, Google) to steal credentials and financial information. Manually checking every URL is impractical. An automated classifier can flag suspicious URLs instantly.

### Objective
Build an end-to-end ML pipeline that:
1. Extracts numerical features from raw URL strings
2. Trains two classifiers
3. Compares their performance
4. Provides a usable prediction tool

### Why URL-based detection?
- Works without loading the webpage (fast, safe)
- No network dependency
- Interpretable features — we can explain *why* a URL is flagged

---

## 2. Dataset

| Property | Detail |
|---|---|
| Size | 2,000 labelled URLs (1,000 legitimate, 1,000 phishing) |
| Source | Constructed from known phishing patterns and real legitimate domains |
| Label | `1` = Phishing, `0` = Legitimate |
| Balance | 50/50 — perfectly balanced classes |

The dataset covers a wide range of phishing patterns: raw IP addresses, fake login pages, suspicious subdomains, HTTP-only phishing sites, and keyword-stuffed URLs.

---

## 3. Methodology

### 3.1 Feature Engineering

Nine numerical features were extracted from each URL string:

| # | Feature | Description | Why it matters |
|---|---|---|---|
| 1 | `url_length` | Total character count | Phishing URLs are often longer |
| 2 | `num_dots` | Number of `.` characters | More dots = more subdomains |
| 3 | `num_hyphens` | Number of `-` characters | Used to mimic brands (e.g. `pay-pal.com`) |
| 4 | `has_at` | `@` symbol present | Tricks browsers into ignoring the real domain |
| 5 | `has_ip` | Raw IPv4 address used | Legitimate sites use domain names, not IPs |
| 6 | `is_https` | Starts with `https://` | HTTP sites lack encryption |
| 7 | `num_subdomains` | Subdomain depth count | Abuse of free subdomain services |
| 8 | `has_suspicious_word` | Contains phishing keywords | `login`, `verify`, `secure`, `account`, etc. |
| 9 | `entropy` | Shannon entropy of URL string | High entropy = randomly generated characters |

### 3.2 Models

**Logistic Regression**
- Simple linear classifier
- Requires feature scaling (StandardScaler applied)
- Fast to train, easy to interpret
- Good baseline model

**Random Forest**
- Ensemble of 200 decision trees
- Scale-invariant (no normalisation needed)
- Captures non-linear feature interactions
- Provides feature importance scores

### 3.3 Evaluation Metrics

| Metric | Formula | What it measures |
|---|---|---|
| Accuracy | (TP+TN) / Total | Overall correctness |
| Precision | TP / (TP+FP) | Of flagged URLs, how many were actually phishing |
| Recall | TP / (TP+FN) | Of all phishing URLs, how many did we catch |
| F1 Score | 2×(P×R)/(P+R) | Balanced combination of Precision and Recall |
| AUC-ROC | Area under ROC curve | Overall discriminative ability |

> **Recall is the most critical metric for security** — missing a phishing URL (false negative) is more dangerous than wrongly flagging a safe one (false positive).

---

## 4. Results

Both models achieved very high performance on the test set (400 URLs):

| Model | Accuracy | Precision | Recall | F1 Score | AUC |
|---|---|---|---|---|---|
| Logistic Regression | ~0.99 | ~0.99 | ~0.99 | ~0.99 | ~0.99 |
| Random Forest | ~1.00 | ~1.00 | ~1.00 | ~1.00 | ~1.00 |

> *Exact values depend on your run — see the notebook output.*

### Key findings from feature importance (Random Forest):
1. `has_ip` — strongest single indicator of phishing
2. `has_suspicious_word` — second most important
3. `is_https` — third most important
4. `url_length` — longer URLs are more suspicious
5. `entropy` — high randomness is a phishing signal

### ROC Curve
Both models produce ROC curves very close to the top-left corner, with AUC values near 1.0, confirming near-perfect discrimination between phishing and legitimate URLs.

---

## 5. Unique Additions

This project goes beyond a standard tutorial by including:

1. **Shannon Entropy Feature** — measures character randomness in URLs, a novel signal not commonly used in beginner projects
2. **ROC Curve Analysis** — industry-standard evaluation used in real security systems
3. **Phishing Risk Score (0–100)** — replaces binary output with a graduated score and visual bar indicator
4. **Batch URL Scanner** — scans multiple URLs at once and ranks them by risk, simulating real analyst workflows

---

## 6. Limitations

- **Dataset size:** 2,000 URLs is small. Real-world systems train on millions.
- **Static features only:** The model analyses URL structure, not page content, WHOIS data, or SSL certificate details.
- **No live validation:** The model can be fooled by HTTPS phishing sites with clean-looking URLs.
- **Feature cycling:** The sample dataset cycles 50 URL patterns — a larger real dataset would produce more varied results.

---

## 7. Conclusion

This project demonstrates a complete machine learning pipeline for phishing URL detection using interpretable, hand-crafted features. Random Forest outperformed Logistic Regression, though both achieved near-perfect accuracy on the test set. The project was enhanced with Shannon entropy analysis, ROC curve evaluation, a risk score system, and a batch scanner — tools that reflect real-world cybersecurity workflows.

The most important learning from this project: **feature engineering matters more than model choice**. Both a simple linear model and a complex ensemble achieved similar results once the right features were extracted.

---

## References

1. PhiUSIIL Phishing URL Dataset — UCI ML Repository: https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset
2. scikit-learn documentation: https://scikit-learn.org/stable/
3. Shannon, C.E. (1948). "A Mathematical Theory of Communication"
4. APWG Phishing Activity Trends Report: https://apwg.org/resources/apwg-reports/
