# Semiconductor-Equipment-Parameter-Correlation-Automated-anomaly-interdiction-SPC-

##  Project Overview
This project simulates a real-world semiconductor equipment data analysis workflow. By processing the high-dimensional **SECOM dataset (1567 examples, 590 sensor features)**, this repository establishes a systematic troubleshooting framework to isolate root causes of yield failures and implement automated anomaly interdiction using Statistical Process Control (SPC).

##  Methodology & Data Engineering

### 1. High-Dimensional Data Cleaning
Industrial sensor data is notoriously noisy. The initial processing pipeline includes:
* **Variance Thresholding:** Automatically identified and removed 116 constant features (variance = 0) that provide no statistical value.
* **Missing Value Imputation (NaN Handling):** Dropped 28 severely corrupted parameters (>50% missing data) and applied **Median Imputation** for the remaining variables to preserve statistical robustness against extreme outliers.
* **Final Dimension:** Reduced the feature space from 590 to 446 highly available parameters.

### 2. Root Cause Analysis (Feature Selection)
Conducted **Pearson Correlation** analysis to evaluate the linear relationship between the 446 sensors and the target Yield Label (104 Fails). 
* Extracted the **Top 5 critical parameters** (highest absolute correlation).
* *Insight:* The maximum correlation coefficient observed was ~0.15, indicating that equipment failures in this dataset are driven by highly complex, non-linear multi-variate interactions rather than single-parameter linear drifts.

### 3. Automated Anomaly Interdiction (3-Sigma SPC)
Developed a vectorized Boolean indexing algorithm in Python (Pandas) to simulate real-time machine alarms:
* Calculated $\mu$ and $\sigma$ for the Top 5 critical parameters.
* Established Upper/Lower Control Limits (UCL/LCL) at **$\mu \pm 3\sigma$**.
* Visualized multi-sensor trends via **Matplotlib**, clearly demarcating the control boundaries and highlighting historical outliers for equipment engineers.

##  Results & Critical Insights

A rigorous intersection analysis was performed to evaluate the effectiveness of the 3-Sigma defense line against the actual 104 historical failures:

* **Total Alarms Triggered:** The Top 5 sensors triggered alarms on 91 unique wafers.
* **True Positives (Hit Count):** 19 actual failed wafers were successfully intercepted.
* **Recall Rate:** **18.27%**

###  Engineering Conclusion
The 18.27% recall rate objectively proves a fundamental limitation of traditional manufacturing: **Univariate 3-Sigma SPC limits are insufficient for intercepting high-dimensional equipment faults.** While SPC successfully blocks extreme linear deviations (catching 19 critical fails), the remaining missed failures (False Negatives) and high False Positive rate underscore the necessity of transitioning from traditional SPC to non-linear Machine Learning classifiers (e.g., SVM, Random Forest) for next-generation yield prediction.

##  Tech Stack
* **Language:** Python 3
* **Libraries:** Pandas (Vectorized Data Processing), Matplotlib (Visualization), NumPy

