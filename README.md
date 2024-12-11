# JosueKaggle

---

# **Kaggle Tabular Project: Santander Product Recommendation**

## **Project Overview**
- **Challenge Link:** [Santander Product Recommendation](https://www.kaggle.com/competitions/santander-product-recommendation/overview)  
- **Description:**  
  In this competition, the objective is to predict which new financial products Santander customers will purchase in the last month of the dataset, 2016-06-28, based on 1.5 years of customer behavior data. The products being predicted are listed in columns #25 - #48 of the dataset.  
  The data is time-split, with training and test datasets separated chronologically. Public and private leaderboard datasets are split randomly.  
- **Dataset Description:**  
  The dataset contains monthly records of customer activity, demographic information, and product holdings. Features include customer age, gender, income, and activity indices. The target is to predict additional product purchases from 24 possible options.

---

## **Data Loading and Initial Look**
- **Files Provided:**  
  - `train.csv` – Training dataset.  
  - `test.csv` – Test dataset for generating predictions.  
  - `sample_submission.csv` – Sample file in the correct submission format.  

- **Data Summary:**  
  - Number of Rows: Nearly 100k
  - Number of Features: 55
 
 

- **Key Features:**  

| **Feature Name**         | **Type**     | **Description**                                         | **Missing Values** | **Notes**                    |
|--------------------------|--------------|---------------------------------------------------------|--------------------|------------------------------|
| `pais_residencia`        | Categorical  | Customer's country of residence                        | None               | Useful for regional patterns |
| `sexo`                   | Categorical  | Customer's gender                                      | Some missing       | Requires imputation          |
| `age`                    | Numerical    | Customer age                                           | Some missing       | Needs rescaling/imputation   |
| `antiguedad`             | Numerical    | Length of customer relationship (in months)           | Some missing       | High variance; requires scaling |
| `canal_entrada`          | Categorical  | Channel through which the customer was acquired        | Some missing       | Indicates customer engagement |
| `cod_prov`               | Categorical  | Province code for customer residence                  | None               | Proxy for geographic trends  |
| `renta`                  | Numerical    | Gross household income                                 | Some missing       | High variance across records |
| `segmento`               | Categorical  | Customer segmentation based on income/activity         | None               | Potentially strong predictor |

## **Data Visualization**
- **Feature Insights:**  
  - Histograms were created to visualize feature distributions across customer segments and product holding patterns.  
  - Correlations between features were analyzed using a heatmap.  
  - Feature importance visualized from the MultiOutputClassifier class in scikit-learn which is used to train a classifier that can predict multiple target variables.
---

## **Data Cleaning and Preparation**
1. **Missing Value Handling:**  
   - Imputed missing `age` values with the median.  
   - Imputed missing `renta` values using median values grouped by segmentation (`segmento`).  
2. **Rescaling:**  
   - Applied Min-Max scaling to `age` and `renta` to ensure consistency for modeling.  
3. **Feature Encoding:**  
   - One-hot encoded categorical variables such as `sexo` and `indrel_1mes`.  
   - Dropped redundant columns (e.g., `cod_prov` and `nomprov`).  

---

## **Machine Learning**

### **Problem Formulation**
- Removed unnecessary columns such as identifiers (`ncodpers`) and redundant features after encoding.  
- Target variable columns include all 24 product columns (`ind_ahor_fin_ult1` to `ind_recibo_ult1`).

### **Data Splits**
- Training Data: 80%  
- Validation Data: 20%  

### **Algorithm**
- **Model Used:** XGBoost  
- **Hyperparameter Tuning:**  
  - `max_depth`: 6  
  - `learning_rate`: 0.1  
  - `n_estimators`: 100  
  - Other parameters optimized using grid search.  

### **Performance**
- **Validation MAP@7 Score:** 0.04
  - Custom function used to calculate MAP@7, aligning with Kaggle competition requirements.  
- **Observations:**  
  - The model's score is .01 higher than other submissions which means there could be an issue in the evaluation function or the way predictions are being calculated

---
