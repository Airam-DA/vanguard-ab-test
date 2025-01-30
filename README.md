![v_banner](https://github.com/user-attachments/assets/d5d90981-304b-4eb7-9c82-d2a777a59211)

# **Transforming User Experience: Vanguard A/B Test Results**
This repository contains the analysis of an A/B test conducted by Vanguard to evaluate the effectiveness of a redesigned user interface (UI). The new UI introduced modern design elements and in-context prompts to enhance the user experience. This project explores the experiment's results, focusing on user behavior, completion rates, and efficiency improvements.

## Context & Study Design

This test was conducted to address a major pain point: Vanguard clients often abandon an essential online process— not due to a lack of intent, but because the interface frustrates them. These moments of friction impact efficiency, client satisfaction, and long-term revenue.

### Experiment Overview:
- **Duration**: March 15, 2017 – June 20, 2017
- **Control Group**: Used Vanguard's traditional online process.
- **Test Group**: Experienced the redesigned, modern interface.
- **Process Flow**: An initial page, three steps, and a confirmation page.

**Objective**: Determine if the updated UI improves completion rates and enhances the user experience.

![client_overview_pb](https://github.com/user-attachments/assets/4a0e1d43-a2d9-4181-ba59-9cbe8e83f7c4)

---

# 📁 Table of Contents
 
1. [📈 A/B Test Raw Data Exploration](#a-b-test-raw-data-exploration)  
2. [✅ Data Cleaning & Preprocessing](#data-cleaning--preprocessing)  
3. [🎨 Exploratory Data Analysis (EDA) Key Insights](#exploratory-data-analysis-eda-key-insights)  
4. [✨ KPI Insights](#kpi-insights)  
5. [📊 A/B Testing Statistical Analysis](#a-b-testing-statistical-analysis)  
6. [📄 Files in this Repository](#files-in-this-repository)  
7. [💻 Technical Stack](#technical-stack)  
8. [🤝 Contributions](#contributions)  
9. [📝 License](#license)  

---

# 📈 A/B Test Raw Data Exploration
The raw data consisted of three datasets capturing demographic, behavioral, and web interaction data:

1. **`df_demo` (Client Demographics and Behavior Data)**
   - Provides insights into client demographics, account details, and recent activity.
     - `client_id`: Unique client identifier
     - `clnt_tenure_yr`, `clnt_tenure_mnth`: Client tenure
     - `clnt_age`: Client age
     - `gendr`: Gender
     - `num_accts`: Number of accounts
     - `bal`: Account balance
     - `calls_6_mnth`, `logons_6_mnth`: Recent client activity

2. **`df_experiment_clients` (Experiment Group Data)**
   - Identifies which group (control or test) each client belongs to.
     - `client_id`: Unique client identifier
     - `variation`: Test or control group assignment

3. **`df_web_data_pt_1` and `df_web_data_pt_2` (Web Interaction Data)**
   - Captures client interactions with the online process.
     - `client_id`: Unique client identifier
     - `visitor_id`: Client-device identifier
     - `visit_id`: Web session identifier
     - `process_step`: Step in the online process
     - `date_time`: Timestamp of interaction

---

# ✅ Data Cleaning & Preprocessing

### 1. **Merging and Consolidation**
   - Unified `df_web_data_pt_1` and `df_web_data_pt_2` into a single DataFrame using `pd.concat()`.
   - Merged demographic and experiment data (`df_demo` and `df_experiment_clients`) using an outer join on `client_id`.
   - Combined web interaction and demographic data into a final DataFrame (`df`) using a left join on `client_id`.

### 2. **Handling Missing Values**
   - Dropped rows with missing `control_test` values (~57% of rows).
   - Filled missing demographic and numeric values with 0.
   - Assigned "U" (Unknown) to missing gender values.

### 3. **Standardizing and Cleaning Columns**
   - Renamed columns to follow a consistent format: lowercase, underscores instead of spaces.
   - Organized columns for usability.

### 4. **Removing Duplicates**
   - Identified and removed duplicate rows based on `session_id`, `client_dev_id`, `process_step`, and `date_time`.

### 5. **Data Type Adjustments**
   - Converted `date_time` to a datetime format.
   - Adjusted numeric columns to appropriate types.

### 6. **Enhancements and Segmentations**
   - **Process Step Sorting**:
     - Added numeric prefixes to `process_step` (e.g., `start` → `0_start`) to ensure proper sorting. 
     - Converted `process_step` into an ordered categorical column for analysis.
   - **Age Groups**:
     - Segmented clients into age ranges (e.g., 16-30, 31-40).
   - **Balance Segments**:
     - Categorized balances into meaningful ranges (e.g., 0-50k, 50-150k).

---

# 🎨 Exploratory Data Analysis (EDA) Key Insights

## **Demographics**
- The **average client age is 48.5 years**, with a wide range from **16 to 96 years**. The age distribution is similar between groups, with the majority of users between 25 and 65 years old.
  Performance was similar through the process steps, indicating that age was not a differentiating factor.

![1_Age Group Distribution Control vs Test](https://github.com/user-attachments/assets/78f1be19-6e44-4922-bacf-ab35bb4afb87)

![2_Age Distribution by Control:Test and Process Step](https://github.com/user-attachments/assets/a81ed3ba-5210-4c19-8ea1-73d2ff9bea9d)

- **Gender distribution is fairly even** and shows no impact on performance: **34% unknown**, **34% male**, **32% female**.

![3_Gender Distribution by Control:Test Group](https://github.com/user-attachments/assets/bb911f70-1090-446b-9b3c-ee34a0e73972)

![4_Gender Distribution by Process Step](https://github.com/user-attachments/assets/17a192dd-f4e0-4442-aa06-fc56880528e3)

## **Balance Distribution**
- The average client holds **2 accounts** with a **mean total balance of $160,747**, though the data is heavily skewed by high balances.
- The **median balance is $69,056**, with most clients falling between **$24,000 and $842,000 (97.5% range)**.
- The **maximum balance reaches $16.3M**, highlighting significant outliers.
- The **test group exhibits more skewness** compared to the control.

![5_Balance Segment Distribution](https://github.com/user-attachments/assets/5fb25c0a-16d3-4ca3-ad65-494d2f3f533d)

![6_Balance Distribution by Control:Test Group](https://github.com/user-attachments/assets/402b589a-d425-429e-8abe-5810e3fb6903)

## **Account Relationships**
- Clients **typically hold 2 accounts** (75% of clients).
- The number of accounts ranges from **1 to a maximum of 7**.

## **Tenure**
- The average client has been with Vanguard for **12 years**, with a maximum tenure of **55 years**.
- The **distribution of tenure is identical** between control and test groups.

![7_Tenure in Years by Control:Test Group](https://github.com/user-attachments/assets/8934ad8b-5e42-42b4-acd3-74401f37a404)

## **Digital Engagement**
- Clients log on an average of **6 times over 6 months**.
- **75% of clients log on 9 or fewer times**, with the **top quartile being highly active (9+ logons)**.

## **Support Needs**
- Clients make an average of **3.2 calls per 6 months**.
- **75% of clients make 6 or fewer calls**.
- There’s a **strong correlation between logons and calls (Pearson = 0.99)**, with **logons occurring roughly twice as often as calls** (mean: **6.28 vs. 3.23**).
- The distribution is **consistent across control and test groups**.

![8_Number of Logins and Calls in the Last 6 Months](https://github.com/user-attachments/assets/f07a0b89-e184-45ad-8648-c413e7d12576)

## **Step Progression Efficiency**
- **Test group users progress more consistently** through the process, particularly in the **final step**.

![9_Funnel Chart  User Progress (Control vs Test)](https://github.com/user-attachments/assets/092431b5-aaa1-4068-9c43-1a917b43fa7f)

---

# ✨ KPI Insights

![kpis_pb](https://github.com/user-attachments/assets/488ac391-2d92-4859-bb57-46a5fc80ab27)

#### 1. **Completion Rate**
   - **Definition**: The number of users who reached the ‘confirm’ step divided by the total number of users in that group.
   - **Control**: 49.85%  
   - **Test**: 58.52%  
   - **Insight**: The Test group shows a notable increase in completion rate compared to the Control group, which indicates that the changes implemented in the Test version had a positive impact on user engagement and the likelihood of completing the process.

![10_Step Completion Rates  Control vs Test Group](https://github.com/user-attachments/assets/9f5769b2-a42a-479c-85e2-eca02f047530)

#### 2. **Average Time per Step (s)**
   - **Definition**: The average time spent by users on each process step, measured in seconds.
   - **Control**: 96.68 seconds  
   - **Test**: 93.17 seconds  
   - **Insight**: Users in the Test group take slightly less time per step, suggesting improved efficiency. This could be a result of a more intuitive process flow or interface, leading to faster task completion.

#### 3. **Error Rate**
   - **Definition**: The percentage of users who move backward in the process, indicating errors or confusion.
   - **Control**: 19.08%  
   - **Test**: 24.26%  
   - **Insight**: The Test group has a higher error rate, which is a point of concern. This could be indicative of issues with the new interface or process flow, possibly creating confusion that leads to more errors, despite the higher completion rate.

#### 4. **Drop-Off Rate**
   - **Definition**: The percentage of users who leave the process at each step.
   - **Control**: 32.31%  
   - **Test**: 30.06%  
   - **Insight**: The Test group demonstrates a lower drop-off rate, suggesting that users in the Test group are less likely to abandon the process. This reflects better retention, which could be attributed to the more engaging or user-friendly design in the Test group.

#### 5. **Completion Rates by Balance Segment**
   - **Definition**: The completion rates segmented by balance categories, showing how different user groups (based on account balance) perform in the process.
   - **Insight**: The Test group generally performs better than the Control group in terms of completion rates, especially in the lower to mid-balance segments (0-500k). However, the 5M+ balance segment shows the lowest completion rate in the Test group (50%), suggesting that high-net-worth clients face challenges, even with the changes tested.

![11 Completion Rate by Balance Segment](https://github.com/user-attachments/assets/70126ffe-97bb-4986-a16b-5d73fbc31618)

---

### **Overall Summary**  
- **Positive**: The Test group shows higher completion rates, faster task completion, and lower drop-offs, indicating that the changes introduced are generally favorable for user experience.  
- **Improvement Needed**: The higher error rate in the Test group should be investigated further to identify and resolve potential usability issues that may be affecting the overall user experience.
  
![12 A:B Test Results for Major KPIs](https://github.com/user-attachments/assets/256a213c-94bf-4d15-85cc-e86ba7da1858)

---

# 📊 A/B Testing Statistical Analysis   

This analysis evaluates the effectiveness of the new user interface (Test group) compared to the existing design (Control group) through statistical hypothesis testing.  

## 🔹 Key Objective: **Completion Rate Impact**  
The **primary goal** is to determine whether the **new UI significantly improves completion rates** while maintaining usability and business viability.  

## 📌 Key Hypothesis Tests  
### **1️⃣ Completion Rate Analysis (Primary Focus)**
#### **Standard Completion Rate Test (Two-Proportion Z-Test)**
- **H₀**: No significant difference in completion rates.  
- **H₁**: The new UI significantly improves completion rates.  
- **Result**: The new design **significantly improves** completion rates (**p < 0.05**).  

#### **Completion Rate vs. Business Threshold (One-Sided Z-Test)**
- Tests if the Test group’s **completion rate improvement meets/exceeds the 5% business viability threshold**.  
- **Result**: The Test group surpasses the threshold (**p < 0.05**), supporting rollout.

![13_H1_Threshold_Completion Rates for C:T](https://github.com/user-attachments/assets/e0ae940a-0e4c-45dc-b324-d308818fb816)

![14_H1_ZStatistics for Completion Rate](https://github.com/user-attachments/assets/6979b5ca-c8f5-4f6e-9571-0e3ee0bf4743)

### **2️⃣ Error Rate Analysis (User Backward Navigation)**
- **H₀**: No significant difference in error rates (users moving backward).  
- **H₁**: The new UI has a different error rate.  
- **Result**: **No statistically significant difference** (**p = 0.5685**), indicating usability remains stable.  

### **3️⃣ Client Balance Comparison (T-Test)**
- **H₀**: No significant difference in client balance between groups.  
- **H₁**: Significant balance difference exists.  
- **Result**: A **statistically significant difference in balance** (**p = 0.0048**), requiring further analysis.  

### **4️⃣ Client Tenure Analysis (Welch’s T-Test)**
#### **Tenure by Month**  
- **H₀**: No significant difference in tenure (months).  
- **H₁**: Significant difference in tenure exists.  
- **Result**: **No significant difference** (**p = 0.4578**).  

#### **Tenure by Year**  
- **H₀**: No significant difference in tenure (years).  
- **H₁**: Significant difference in tenure exists.  
- **Result**: **No significant difference** (**p = 0.5677**).  

## 💡 Key Takeaways  
✅ **Completion rates significantly improve** with the new UI.  
✅ The **improvement surpasses the 5% business viability threshold**, supporting adoption.  
✅ **Error rates remain stable**, meaning no usability concerns.  
✅ **Balance differences** exist and may require further exploration.  
✅ **No significant tenure differences**, suggesting user engagement is unaffected.  

## 🚀 Conclusion  
The **new UI is a clear success**, showing **statistically and practically significant improvements** in completion rates.  
A full rollout is supported, with potential further analysis on **client balance** impacts.  

---

# 📄 Files in this Repository

| **Category**  | **File Name**                                    | **Description**                                        |
|---------------|--------------------------------------------------|--------------------------------------------------------|
| **Data**      | `raw_data`                                       | Unprocessed datasets from the experiment               |
|               | `clean_vanguard.csv`                           | Cleaned and transformed datasets                         |
| **Notebooks** | `1_vanguard_cleaning_and_wrangling.ipynb`        | Data exploration, cleaning & wrangling                 |
|               | `2_vanguard_EDA.ipynb`                           | Exploratory data analysis                              |
|               | `3_vanguard_KPIs.ipynb`                          | KPI analysis                                           |
|               | `4_vanguard_hypotheses.ipynb`                    | Statistical hypothesis testing                         |
| **Visuals**   | `EDA_visuals`                                    | Generated charts and graphs                            |
|               | `power_bi`                                       | Power BI exports/screenshots                           |
| **Presentation** | `vanguard_ab_test.pdf`                        | Presentation for executives                            |

--- 

# 💻 Tech Stack 

## **Data Manipulation**  
- **pandas**: Data manipulation and analysis with DataFrames.  
- **numpy**: Scientific computing with multi-dimensional arrays.  
- **datetime**: Date and time handling.  

## **Visualization**  
- **matplotlib**: Static, animated, and interactive plots.  
- **seaborn**: Statistical graphics built on matplotlib.  
- **plotly**: Interactive plots and dashboards.  

## **Statistical Analysis**  
- **statsmodels**: Statistical models and hypothesis testing.  
- **scipy.stats**: Statistical tests and distributions.  
- **scipy.stats.contingency**: Categorical variable association.  
- **statsmodels.stats.proportion**: Proportion hypothesis tests.  
- **scipy.stats.kurtosis**: Computes dataset kurtosis.  
- **scipy.stats.probplot**: Creates probability plots.  
- **scipy.stats.chi2_contingency**: Tests categorical independence.  

---

## ⚙️ Installation  

To set up the project, install the required libraries using `pip`:  

    pip install pandas numpy matplotlib seaborn plotly statsmodels scipy

---

# 🤝 Contributions
Contributions are welcome! Feel free to submit issues or pull requests to improve the project.

---

# 📝 License
This project is licensed under the **MIT License**, allowing you to share, modify, and use the code for both personal and professional purposes.
