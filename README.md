# Beyond Behaviour: Detecting Insider Threats with Machine Learning

> A Machine Learning Framework for Insider Threat Detection Using
> Behavioural, Organisational, and Semantic Features

## Overview
Insider threats are an important cybersecurity problem because malicious user activities can look similar to normal employee behaviour. This makes insider threats difficult to detect using only traditional security methods. Using different types of information about user behaviour may help improve the detection of suspicious activities.

This study developed a machine learning framework for insider threat detection using the CERT Insider Threat Dataset (version 6.2). The framework uses behavioural, organisational, and semantic information from user activities and email data. User activities and email content were grouped by user and week, and different combinations of features were tested to examine whether adding different types of information could improve detection. Anomaly detection methods and different detection thresholds were also evaluated because malicious activities are very rare in the dataset.

The results show that behavioural information provided stronger signals for detecting malicious activities than semantic information alone. Combining different types of features gave a broader view of user behaviour, but detection remained difficult because of the large imbalance between normal and malicious activities. The threshold analysis also showed that detecting more malicious activities often resulted in a large number of normal activities being incorrectly classified as suspicious.

These findings show that insider threat detection is difficult when malicious activities are rare and can appear similar to normal behaviour. No single type of information was enough to provide reliable detection. The framework developed in this study provides a way to combine different sources of user information and evaluate their usefulness for insider threat detection, while also showing the importance of choosing an appropriate detection threshold.

## Methods
Raw data inputs (email.csv, LDAP files, and the answer file) were first cleaned and preprocessed, including timestamp processing and weekly aggregation, so that user activities and email content could be grouped by user and week. Feature engineering was then applied to derive two main categories of information: **metadata** and **semantic data**.

- **Metadata features**: Split into behavioural features (patterns derived from user activity logs) and organisational features (information related to the user's role or position within the organisation).
- **Semantic data**: Derived from email content using two approaches — a TF-IDF representation reduced via Truncated SVD, and LLM-based embeddings summarised through semantic deviation.

These feature sets were combined into a final user-week dataset, allowing different combinations of behavioural, organisational, and semantic information to be tested to examine whether adding different types of information could improve detection.

- **Data splitting and validation**: StratifiedGroupKFold cross-validation, grouped by user, was used to prevent data leakage between training and test sets while accounting for the rarity of malicious activities.
- **Models**: Logistic Regression, Isolation Forest, and a Deep Autoencoder were used to detect anomalous user-week behaviour, covering both supervised and unsupervised anomaly detection approaches.
- **Evaluation metrics**: Precision, Recall, F1-score, PR-AUC, and ROC-AUC were used to evaluate model performance, given the strong class imbalance between normal and malicious activities.
- **Threshold selection**: Different detection thresholds were evaluated to examine the trade-off between detecting more malicious activities and increasing false positives among normal activities.

## Data Availability
This project uses the **CERT Insider Threat Dataset (v6.2)**, a synthetic dataset developed by the CERT Division of Carnegie Mellon University's Software Engineering Institute (SEI). The dataset provides realistic background activity for normal users alongside injected malicious actor scenarios, avoiding the privacy and labelling issues associated with real organisational data.

This project uses release **r6.2**, the most recent and extensive version, containing data from 4,000 users. An accompanying answers file lists the malicious scenarios and the identifiers of the users involved, providing ground truth labels for evaluation.

Due to access and reuse restrictions, the dataset is **not included** in this repository. It can be downloaded from the CERT Insider Threat Test Dataset page hosted by Carnegie Mellon University:

- [Insider Threat Test Dataset – Carnegie Mellon University](https://kilthub.cmu.edu/articles/dataset/Insider_Threat_Test_Dataset/12841247)

Make sure to download **version r6.2** specifically, then extract the compressed data folder to access the log files (`email.csv`, LDAP files, etc.) before running the preprocessing scripts. It is recommended to extract and inspect the dataset structure before starting any analysis.

## Repository Structure
- `report/`: Final report (LaTeX source files and figures)
- `code/`: Analysis scripts and models
  - `preprocessing/`: Data cleaning, timestamp processing, and weekly aggregation
  - `features/`: Feature engineering (behavioural, organisational, and semantic features)
  - `models/`: Logistic Regression, Isolation Forest, and Deep Autoencoder implementations
  - `evaluation/`: Model evaluation and threshold selection scripts
- `data/`: Raw data (not included in repo — see [Data Availability](#data-availability))
- `README.md`: Project overview and documentation