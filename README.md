Cardiac Biomarker Predictive Analysis

1. Introduction
   
The Medicaldataset.csv dataset comprises 1319 patient records, each characterized by nine variables: Age (in years), Gender (0 = Female, 1 = Male), Heart Rate (beats per minute), Systolic Blood Pressure (mmHg), Diastolic Blood Pressure (mmHg), Blood Sugar (mg/dL), CK-MB (ng/mL), Troponin (ng/mL), and Result (a binary outcome: positive or negative, likely indicative of a cardiac condition such as myocardial infarction). The objective of this analysis is to explore relationships between these variables and the outcome, identify significant predictors, and develop a predictive model for classifying patients based on their likelihood of a positive result. The analysis is conducted using R, employing a structured approach that include data cleaning, exploratory data analysis (EDA), statistical testing, predictive modeling, and visualization. This report presents the methodology, findings, interpretations, and recommendations.

3. Methodology
   
2.1 Data Cleaning and Preprocessing

The dataset is loaded into R, and an initial quality check revealed no missing values across the 1319 records, ensuring a complete dataset for analysis. However, several extreme values suggested potential data entry errors, such as Heart Rate values of 1111 bpm, which are biologically implausible (normal range: 60–100 bpm, with extremes up to ~200 bpm under stress). To address outliers, the Interquartile Range (IQR) method was applied, capping values beyond 1.5 * IQR from the first and third quartiles for all numeric variables (Age, Heart Rate, Systolic Blood Pressure, Diastolic Blood Pressure, Blood Sugar, CK-MB, Troponin). This approach balanced data integrity with the need to mitigate the impact of extreme values, such as Blood Sugar levels exceeding 500 mg/dL or CK-MB values up to 300 ng/mL, which are rare but plausible in clinical contexts.
Column names is standardized (e.g., “Heart.rate” to “heart_rate”) for consistency, and categorical variables Gender and Result were converted to factors. Gender was recoded as “Female” (0) and “Male” (1), and Result as “negative” and “positive” for binary classification.

2.2 Exploratory Data Analysis (EDA)

To gain insight into the dataset’s structure, summary statistics was computed, and visualizations were generated. A bar plot was created to examine the distribution of the Result variable, boxplots were used to compare Age, Troponin, and CK-MB across Result groups, and a correlation matrix heatmap was produced to assess relationships among numeric variables. These visualizations facilitated the identification of key patterns and relationships.

2.3 Statistical Analysis

Independent t-tests, assuming unequal variances, were conducted to compare the means of numeric variables between positive and negative Result groups. A chi-square test of independence was performed to evaluate the association between Gender and Result. These tests aimed to identify variables with significant differences across outcome groups.

2.4 Predictive Modeling

The dataset was split into 80% training (340 records) and 20% testing (86 records) sets using stratified sampling to preserve the outcome distribution. A logistic regression model was trained to predict Result, incorporating all variables as predictors. To ensure robustness, 5-fold cross-validation was employed, optimizing for the Area Under the ROC Curve (AUC). Model performance was evaluated using a confusion matrix (reporting accuracy, sensitivity, and specificity) and an ROC curve with AUC. Feature importance was assessed by extracting and visualizing the model’s coefficients.

2.5 Visualization and Output

Visualizations, includes bar plots, boxplots, a correlation matrix, an ROC curve, and a feature importance plot

3. Results

3.1 Data Cleaning and Preprocessing

    •   Missing Values: No missing data was detected, confirming a complete dataset.
    •  Outliers: The IQR method identified outliers in several variables:
  	•  Heart Rate: Approximately 2 records with values like 1111 bpm.
  	•  Blood Sugar: Approximately 10 records exceeding 400 mg/dL.
  	•  CK-MB: Approximately 20 records above 50 ng/mL.
  	•  Troponin: Approximately 10 records above 1 ng/mL. Capping these values at IQR bounds minimized their impact while retaining clinical plausibility.

•  Summary Statistics:

  	•  Age: Mean = 56.2 years, median = 58 years, range = 19–103 years.
  	•  Heart Rate: Mean = 75.93 bpm, median = 74 bpm, range = 20–135 bpm.
  	•  Systolic Blood Pressure: Mean = 126.8 mmHg, median = 124 mmHg, range = 42–223 mmHg.
  	•  Diastolic Blood Pressure: Mean = 72.2 mmHg, median = 72 mmHg, range = 38–128 mmHg.
  	•  Blood Sugar: Mean = 149.1 mg/dL, median = 122 mg/dL, range = 50–541 mg/dL.
  	•  CK-MB: Mean = 4.5 ng/mL, median = 2.9 ng/mL, range = 0.35–300 ng/mL.
  	•  Troponin: Mean = 0.06 ng/mL, median = 0.01  ng/mL, range = 0.001–10 ng/mL.
  	•  Gender: 66% Male (870  records), 34% Female (449 records).
  	•  Result: 61.4% Positive (810 records), 38.6% Negative (509 records).

3.2 Exploratory Data Analysis

    •  Result Distribution: A bar plot indicated 810 positive cases (61.4%) and 509 negative cases (38.6%), reflecting a moderate class imbalance.
    •  Age by Result: Boxplots showed that positive cases had a higher median age (~60 years) than negative cases (~55 years), with a wider interquartile range for positive cases.
    •  Troponin by Result: Positive cases exhibited significantly higher Troponin levels (median ~0.03 ng/mL) wcompared to negative cases (~0.008 ng/mL), with several outliers in the positive group.
    •  CK-MB by Result: Positive cases had higher CK-MB levels (median ~3.3 ng/mL) than negative cases (~2.0 ng/mL), with extreme values in the positive group.
    •  Correlation Matrix: A moderate positive correlation was observed between Systolic and Diastolic Blood Pressure (r ≈ 0.45). Other variables showed weak correlations (e.g., Troponin vs. CK-MB, r ≈ 0.15), indicating minimal multicollinearity.

3.3 Statistical Analysis

•  T-tests:

  	•  Age: Significant (p < 0.01), with a mean difference of ~5 years (positive: 58.7 years, negative: 52.1 years).
  	•  Heart Rate: Not Significant (p ≈ 0.9), with a mean difference of ~0.1 bpm.
  	•  Systolic Blood Pressure: Marginally significant (p ≈ 0.4), with a mean difference of ~1.2 mmHg. Not significant 
  	•  Diastolic Blood Pressure and Blood Sugar: Not significant (p > 0.05).
  	•  CK-MB: Highly significant (p < 0.001), with a mean difference of ~3.1 ng/mL.
  	•  Troponin: Highly significant (p < 0.001), with a mean difference of ~0.1 ng/mL.
    •  Chi-square Test: A significant association was found between Gender and Result (p ≈ 0.001), with males more likely to have positive results.

3.4 Predictive Modeling

    •  Logistic Regression Model:
    •  Significant Predictors:
    •  Troponin: Strongest predictor (p < 0.001).
    •  Gender: significant (p < 0.001).
    •  CK-MB: Significant (p < 0.05).
    •  Age: Significant (p < 0.005).
    •  Non-significant Predictors: Heart Rate, Systolic Blood Pressure, Diastolic Blood Pressure, Blood Sugar (p > 0.05).
 
  •  Model Performance (test set):
  
    •  Accuracy: ~93.2%% (244/264 cases correctly classified).
    •  Sensitivity: ~95.2%% (164/180 positive cases correctly identified).
    •  Specificity: ~91.1%% (80/84 negative cases correctly identified).
    •  AUC: ~97.5%, indicating strong discriminative ability.
    •  Feature Importance: Troponin and CK-MB exhibited the largest coefficients, underscoring their critical role in prediction.

3.5 Visualizations

    •  Bar Plot: Highlighted the moderate imbalance in Result (58.5% positive).
    •  Boxplots: Demonstrated higher Age, Troponin, and CK-MB levels in positive cases.
    •  Correlation Matrix: Confirmed moderate correlation between blood pressure variables.
    •  ROC Curve: Illustrated the model’s strong performance (AUC ≈ 0.98).
    •  Feature Importance Plot: Visualized Troponin and CK-MB as the dominant predictors.

4. Discussion

4.1 Key Findings

This analysis provides several insights into the medical dataset and its implications for cardiac diagnosis:

    1.  Cardiac Biomarkers: Troponin and CK-MB are the strongest predictors of a positive result, with significantly higher levels in positive cases (p < 0.001). This is consistent with their established role in diagnosing myocardial infarction, as elevated levels indicate myocardial injury.
    2.  Age and Gender: Older patients (mean ~56 years) and males (66% positive rate) are more likely to have positive outcomes, aligning with known cardiovascular risk factors.
    3.  Vital Signs and Blood Sugar: Heart Rate and Systolic Blood Pressure showed marginal differences, but Diastolic Blood Pressure and Blood Sugar were not significant predictors, suggesting limited diagnostic value in this context.
    4.  Model Performance: The logistic regression model achieved ~92% accuracy and an AUC of 0.98, indicating good but not exceptional performance. Troponin and CK-MB dominated predictions, while vital signs contributed minimally.
    5.  Data Quality: Outliers, such as Heart Rate values of 1111 bpm, were likely data entry errors. Capping these values ensured robust analysis, though some extreme Blood Sugar and CK-MB values were retained as clinically plausible.

4.2 Clinical Implications

The prominence of Troponin and CK-MB suggests that clinical protocols should prioritize these biomarkers for diagnosing cardiac conditions. Age and Gender can enhance risk stratification, as older males appear at higher risk. The lack of predictive power from Blood Sugar indicates that diabetes status may not directly influence the outcome in this dataset, though its high variability warrants further exploration.

4.3 Strengths

         •  Robust Data Cleaning: The IQR-based outlier capping balanced data integrity and analysis stability.
         •  Comprehensive Analysis: The integration of EDA, statistical tests, and predictive modeling provided a holistic understanding of the dataset.
         •  Cross-Validation: 5-fold cross-validation ensured reliable model performance estimates.
         •  Clear Visualizations: Plots effectively communicated key patterns, such as the strong association between biomarkers and outcomes.

4.4 Limitations

    1.  Class Imbalance: The moderate imbalance (61.4% positive) may bias the model toward positive predictions.
    2.  Outlier Assumptions: While outliers were capped, some extreme values (e.g., Blood Sugar = 541 mg/dL) were assumed plausible but could reflect measurement errors.
    3.  Model Simplicity: Logistic regression assumes linear relationships, potentially missing complex patterns in the data.
    4.  Lack of Context: The absence of metadata on the specific condition or measurement protocols limits clinical interpretation.

5. Recommendations
   
    5.1. Clinical Application:
     
            •  Prioritize Troponin and CK-MB testing for cardiac diagnosis due to their strong predictive power.
            •  Incorporate Age and Gender into risk assessment models, given their significant influence on outcomes.
        
    5.2.  Data Quality:
   
            •  Implement stricter data validation to prevent errors like Heart Rate values of 1111 bpm.
            •  Verify extreme Blood Sugar and CK-MB values with clinical experts to confirm their validity.
        
    5.3.  Future Analysis:
       
            •  Explore non-linear models, such as random forests or gradient boosting, to capture complex relationships and potentially improve accuracy.
            •  Conduct subgroup analyses, such as comparing diabetic versus non-diabetic patients based on Blood Sugar thresholds.
            •  Incorporate interaction terms (e.g., Troponin * CK-MB) or categorize Age (e.g., <40, 40–60, >60) to enhance model performance.
        
    5.4.  Additional Data: If available, include time-to-event data for survival analysis or additional clinical variables (e.g., ECG results) to improve diagnostic accuracy.

7. Conclusion
   
This analysis of the dataset confirms that Troponin and CK-MB are the most critical predictors of a positive cardiac outcome, with Age and Gender also contributing significantly. The logistic regression model achieved solid performance (92% accuracy, AUC = 0.98), but advanced modeling or feature engineering could further enhance results. Clinically, the findings emphasize the importance of cardiac biomarkers and suggest that older males are at higher risk. Addressing data quality issues and exploring more sophisticated models could improve diagnostic accuracy in future work. This project offered valuable insights into medical data analysis and its clinical applications, highlighting the power of data-driven approaches in healthcare.
