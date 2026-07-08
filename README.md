# Banking System with AI/ML — Project Summary

## Objectives
- Build a simulated end-to-end banking system covering core banking operations: fund transfer, transaction history, KYC, loans, fraud detection, and statements.
- Demonstrate practical application of security and software engineering concepts: encryption, role-based access control (RBAC), and logging.
- Integrate real Machine Learning models (not just rule-based logic) into core banking workflows — fraud scoring, loan approval, and customer segmentation.
- Extend the system toward production-readiness with model persistence, a persistent database ledger, and an NLP-based support chatbot.
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
| Text Classification (TF-IDF + Naive Bayes) | Support chatbot — intent classification for customer queries |
| Model Serialization (joblib) | Saving/loading trained models without retraining |

## Dataset
- **Source:** UCI Bank Marketing Dataset (train.csv — 45,211 rows, test.csv — 4,521 rows)
- **Features:** age, job, marital status, education, default status, account balance, housing loan, personal loan, contact type, day/month of contact, call duration, campaign count, days since previous contact, previous outcome
- **Target (y):** originally "subscribed to term deposit" — repurposed here as a proxy label for loan/financial product approval likelihood
- **Fraud detection data:** synthetic but realistically distributed (amount, hour of day, new beneficiary flag, distance from home, transaction frequency) since no public labeled fraud dataset was provided
- **Chatbot training data:** small hand-crafted set of example customer messages across 7 intents

## Features Implemented
- **Fund Transfer** — with real-time fraud screening before completion
- **Transaction History** — immutable in-memory ledger, queryable per account
- **KYC** — rule-based plus name-similarity verification, gates account opening
- **Loan Module** — ML-based eligibility prediction plus EMI calculator
- **Fraud Detection** — dual-model (Isolation Forest + Random Forest) real-time risk scoring
- **Statements** — filtered transaction views plus balance-over-time visualization
- **Security** — Fernet encryption (PINs), SHA-256 password hashing
- **Role Management** — ADMIN / MANAGER / CUSTOMER / AUDITOR with decorator-enforced permissions
- **Logging** — every action logged to console and bank_system.log
- **Model Persistence** — joblib save/reload for all three trained models
- **Persistent Ledger** — SQLite database (bank_ledger.db) surviving session restarts
- **Support Chatbot** — intent-classified responses backed by live account data

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
| Model Persistence | Reload without retraining | Successful, identical predictions |
| SQLite Ledger | Data read back after connection close/reopen | 7/7 transactions persisted correctly |
| Chatbot | Intent classification on 6 test messages | 6/6 correctly classified and routed |

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
13. Final Results Report — consolidated printout of every outcome
14. Model Persistence — save and reload trained models with joblib
15. Persistent Ledger — sync transactions to a SQLite database file
16. Support Chatbot — train intent classifier, demo live query handling

## Tools Used
- **Language:** Python 3
- **Environment:** Google Colab (Jupyter notebook)
- **ML/Data:** scikit-learn (IsolationForest, RandomForestClassifier, KMeans, StandardScaler, TfidfVectorizer, MultinomialNB), pandas, numpy
- **Security:** cryptography (Fernet), hashlib
- **Visualization:** matplotlib
- **Text Matching:** difflib (standard library)
- **Persistence:** joblib, sqlite3
- **Logging:** Python logging module
- **Core Language Features:** OOP (classes/inheritance), decorators, enums, custom exceptions
