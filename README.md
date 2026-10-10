# Intelligent Student Performance Analytics System

A Big Data PBL project that uses **Apache Spark (PySpark)** to process large-scale university student data, predict academic outcomes, segment students with clustering, and give faculty actionable insights on who needs academic support.

## Problem Statement
Universities generate huge volumes of data from attendance, internal assessments, semester exams, learning activities and course registrations. This project builds a distributed analytics pipeline that:
- Preprocesses and analyzes the data at scale
- Predicts at-risk students and semester marks using scalable ML
- Groups students into performance categories using K-Means
- Visualizes results to help faculty intervene early

## Tech Stack
- **Apache Spark / PySpark** (DataFrames, Spark SQL, Spark MLlib)
- **Python:** pandas, NumPy, scikit-learn (PCA, ROC), matplotlib, seaborn
- **Google Colab** (Spark in local mode, multi-core)

## Dataset
A synthetic dataset of **100,000 students** is generated in a distributed manner using Spark. Features:

| Feature | Description |
|---|---|
| attendance_pct | Attendance percentage |
| internal1, internal2 | Internal assessment scores |
| assignment_score | Assignment performance |
| lms_hours | Learning platform activity (hours) |
| courses_registered | Number of registered courses |
| backlogs | Number of backlogs |
| department | CSE, ECE, ME, CE, IT |
| semester_marks | Final exam marks (regression target) |
| at_risk | 1 if semester_marks < 55 (classification target) |

About 3% missing values are injected into some columns to demonstrate preprocessing. To use real data, replace the generation cell with `spark.read.csv("your_file.csv", header=True, inferSchema=True)`.

## Pipeline
1. **Preprocessing:** missing-value check, median imputation, categorical encoding, feature engineering (`internal_avg`), feature vector assembly
2. **Distributed processing:** partitioned DataFrames, caching, group-by aggregations, Spark ML Pipelines
3. **Classification:** Random Forest and Logistic Regression to predict at-risk students
4. **Regression:** Linear Regression and Random Forest Regressor to predict semester marks
5. **Clustering:** K-Means with elbow and silhouette analysis; segments named Needs Support / Average / Good / High Performer
6. **Visualization:** distribution, correlation heatmap, feature importance, ROC curve, PCA cluster view, segment boxplots
7. **Faculty insights:** list of students with at-risk probability > 0.7 exported to CSV


Best K for clustering: **4** (chosen using elbow and silhouette).

<img width="2700" height="1500" alt="dashboard" src="https://github.com/user-attachments/assets/878d9d14-377c-443a-b848-0174311f7fa2" />


## Key Insights
- Attendance, internal marks and LMS activity are the strongest predictors of performance.
- Students in the "Needs Support" cluster show low attendance, low engagement and a high share of at-risk students.
- Faculty can run the model after Internal 1 to identify students for early intervention.

## How to Run
1. Open `notebooks/student_performance_analytics.ipynb` in [Google Colab](https://colab.research.google.com)
2. Run all cells in order (the first cell installs PySpark)
3. Outputs: `dashboard.png` and `students_needing_support.csv`

Or locally:
```bash
pip install -r requirements.txt
jupyter notebook notebooks/student_performance_analytics.ipynb
```
