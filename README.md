# Banking System with AI/ML — Project Summary

## Objectives
- Build a simulated end-to-end banking system covering core banking operations: fund transfer, transaction history, KYC, loans, fraud detection, and statements.
- Demonstrate practical application of security and software engineering concepts: encryption, role-based access control (RBAC), and logging.
- Integrate real Machine Learning models (not just rule-based logic) into core banking workflows — fraud scoring, loan approval, and customer segmentation.
- Deliver a fully runnable, self-contained Google Colab notebook suitable for demonstration or academic submission.

## ML Concepts Covered
| Concept | Where it's used |
|---|---|
| Unsupervised Anomaly Detection (Isolation Forest) | Fraud Detection — flags statistically unusual transactions without labeled examples |
| Supervised Classification (Random Forest) | Fraud Detection (fraud probability) and Loan Module (approval probability) |
| Feature Engineering and Scaling (StandardScaler) | Preprocessing for fraud scoring and clustering |
| Clustering (K-Means) | Customer Segmentation into Premium / Regular / At-Risk groups |
| One-Hot Encoding | Converting categorical loan-applicant data (job, education, etc.) into model-ready features |
| Train/Test Split and Model Evaluation | Accuracy, precision, recall, F1-score reporting for both ML models |
| NLP-style Text Similarity (difflib) | KYC name-matching between account holder and submitted documents |

## Dataset
- **Source:** UCI Bank Marketing Dataset (train.csv — 45,211 rows, test.csv — 4,521 rows)
- **Features:** age, job, marital status, education, default status, account balance, housing loan, personal loan, contact type, day/month of contact, call duration, campaign count, days since previous contact, previous outcome
- **Target (y):** originally "subscribed to term deposit" — repurposed here as a proxy label for loan/financial product approval likelihood
- **Fraud detection data:** synthetic but realistically distributed (amount, hour of day, new beneficiary flag, distance from home, transaction frequency) since no public labeled fraud dataset was provided

## Features Implemented
- **Fund Transfer** — with real-time fraud screening before completion
- **Transaction History** — immutable ledger, queryable per account
- **KYC** — rule-based plus name-similarity verification, gates account opening
- **Loan Module** — ML-based eligibility prediction plus EMI calculator
- **Fraud Detection** — dual-model (Isolation Forest + Random Forest) real-time risk scoring
- **Statements** — filtered transaction views plus balance-over-time visualization
- **Security** — Fernet encryption (PINs), SHA-256 password hashing
- **Role Management** — ADMIN / MANAGER / CUSTOMER / AUDITOR with decorator-enforced permissions
- **Logging** — every action logged to console and bank_system.log

## Results Summary
| Model | Metric | Result |
|---|---|---|
| Fraud Detector (Random Forest) | Test Accuracy | ~99%+ on synthetic data |
| Fraud Detector (Isolation Forest) | Correctly flagged suspicious demo transaction | HIGH risk (score 1.0) |
| Loan Engine (Random Forest, real data) | Test Accuracy | 87.0% |
| Loan Engine | Precision / Recall (approved class) | 0.47 / 0.91 |
| Customer Segmentation (K-Means) | 3 segments identified | Premium, Regular, At-Risk / Low Balance |
| RBAC demo | Unauthorized cross-account access attempt | Correctly blocked |
| Fraud demo | High-risk transfer (48,000, 2 AM, new beneficiary, 900km away) | Correctly blocked |

## Project Workflow
1. Setup — install dependencies, configure logging
2. Data Loading — load real UCI dataset (with Colab upload fallback)
3. Security Layer — build encryption and password hashing utilities
4. RBAC Layer — define roles and access-control decorator
5. Core Models — User, Customer, Account classes
6. KYC Module — verify identity before account creation
7. Fraud Detection Model — generate data, train, validate
8. Loan Module Model — train on real data, validate, build EMI calculator
9. Segmentation Model — cluster customers, visualize segments
10. Bank Engine — wire together accounts, transfers (with fraud check), ledger, statements
11. End-to-End Demo — simulate registrations, KYC, transfers (normal and fraudulent), loan application, statement generation, RBAC violation
12. Visualization — balance-over-time chart, segment distribution chart

## Tools Used
- **Language:** Python 3
- **Environment:** Google Colab (Jupyter notebook)
- **ML/Data:** scikit-learn (IsolationForest, RandomForestClassifier, KMeans, StandardScaler), pandas, numpy
- **Security:** cryptography (Fernet), hashlib
- **Visualization:** matplotlib
- **Text Matching:** difflib (standard library)
- **Logging:** Python logging module
- **Core Language Features:** OOP (classes/inheritance), decorators, enums, custom exceptions

Want this exported as a formatted PDF or Word document for submission?
