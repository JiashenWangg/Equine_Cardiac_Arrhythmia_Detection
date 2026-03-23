# Detecting Equine Cardiac Arrhythmia from ECG Data

## Background

Cardiac arrhythmia detection using ECG signals is critical but time-intensive. This project develops scalable statistical and machine learning approaches to identify abnormal heart rhythms in horses using RR interval data, with applications in early diagnosis and veterinary decision support.

## Data

Source: Cornell University Equine ECG dataset

Scope: 13 horses, 11 time windows across ~24 hours

Variables: RR intervals (primary), HR, time, derived features

Phases: Pre-injection, severe symptoms, recovery

Scale: 500k+ ECG observations across time windows

## Stage 1: Statistical Feature Engineering & Clustering

### Methods

Outlier Detection: Removed AV blocks using thresholding, ARIMA residuals, and delta-RR filtering

Feature Engineering: Extracted RR summary statistics (mean, variance, SD1, SD2)

Modeling:

Gaussian Mixture Models (GMM) for distributional clustering

Hierarchical clustering (complete linkage)

PCA for dimensionality reduction

### Results

PCA explained ~66.6% variance with first two components

SD1 and max RR strongly associated with arrhythmia phases

Clustering showed clear separation of pre-symptom vs post-symptom states

Limited separation between severe and recovery phases, indicating gradual physiological transition

## Stage 2: Time Series Modeling with Functional PCA

### Methods

Converted discrete windows into continuous time series representations

Standardized RR sequences and resampled into fixed-length vectors (100 time points)

Applied Functional PCA (FPCA) to capture temporal dynamics

Analyzed principal component loadings across time

### Results

First two components captured ~43.7% variance

PC1: Captures overall physiological condition and rapid changes post-injection

PC2: Captures recovery dynamics and long-term trends

Identified critical time windows (0–5h) where arrhythmia effects are most pronounced

Revealed inter-horse variability and temporal evolution patterns

## Stage 3: Supervised Classification with Feature-Based Modeling

### Methods

Reformulated problem as binary classification: baseline vs arrhythmia using 1-hour windows

Split time series into 5-minute chunks (213 samples) to increase sample size and stabilize features

Extracted domain-driven features from Poincaré plots and time series:

SD1, SD2, SD1/SD2, ellipse area

Coefficient of Variation (CV), entropy, max change

Trained interpretable models:

Logistic Regression (baseline)

Lasso-regularized Logistic Regression (feature selection)

Linear Discriminant Analysis (LDA)

Applied Leave-One-Horse-Out Cross Validation to prevent data leakage and ensure generalization

### Results

Best model: Lasso Logistic Regression with strong interpretability and feature selection

Achieved 0.92 AUC-ROC, 0.95 sensitivity, outperforming other models

Key signals identified:

Higher SD1 and CV → arrhythmia (increased short-term variability)

Higher SD2 → healthy rhythm (long-term stability)

Demonstrated that robust feature engineering + simple models outperform complex models under small sample constraints

Built a full pipeline: signal → feature extraction → classification → validation, enabling scalable and interpretable arrhythmia detection

## Key Takeaways

Successfully built an end-to-end pipeline from raw ECG signals → feature engineering → statistical modeling → temporal analysis

Demonstrated strong ability to combine classical statistics + modern ML + time-series methods

Provided actionable insights for early detection and monitoring of horse cardiac abnormalities
