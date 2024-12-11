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

| Column Name                 | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| `fecha_dato`                 | The table is partitioned for this column                                     |
| `ncodpers`                   | Customer code                                                               |
| `ind_empleado`               | Employee index: A active, B ex employed, F filial, N not employee, P pasive |
| `pais_residencia`            | Customer's Country residence                                                |
| `sexo`                       | Customer's sex                                                               |
| `age`                        | Age                                                                          |
| `fecha_alta`                 | The date in which the customer became as the first holder of a contract in the bank |
| `ind_nuevo`                  | New customer Index. 1 if the customer registered in the last 6 months.      |
| `antiguedad`                 | Customer seniority (in months)                                              |
| `indrel`                     | 1 (First/Primary), 99 (Primary customer during the month but not at the end of the month) |
| `ult_fec_cli_1t`             | Last date as primary customer (if he isn't at the end of the month)         |
| `indrel_1mes`                | Customer type at the beginning of the month ,1 (First/Primary customer), 2 (co-owner ),P (Potential),3 (former primary), 4(former co-owner) |
| `tiprel_1mes`                | Customer relation type at the beginning of the month, A (active), I (inactive), P (former customer),R (Potential) |
| `indresi`                    | Residence index (S (Yes) or N (No) if the residence country is the same as the bank country) |
| `indext`                     | Foreigner index (S (Yes) or N (No) if the customer's birth country is different than the bank country) |
| `conyuemp`                   | Spouse index. 1 if the customer is spouse of an employee                    |
| `canal_entrada`              | Channel used by the customer to join                                        |
| `indfall`                    | Deceased index. N/S                                                         |
| `tipodom`                    | Address type. 1, primary address                                            |
| `cod_prov`                   | Province code (customer's address)                                          |
| `nomprov`                    | Province name                                                               |
| `ind_actividad_cliente`      | Activity index (1, active customer; 0, inactive customer)                   |
| `renta`                      | Gross income of the household                                               |
| `segmento`                   | Segmentation: 01 - VIP, 02 - Individuals, 03 - college graduated            |
| `ind_ahor_fin_ult1`          | Saving Account                                                              |
| `ind_aval_fin_ult1`          | Guarantees                                                                  |
| `ind_cco_fin_ult1`           | Current Accounts                                                            |
| `ind_cder_fin_ult1`          | Derivada Account                                                            |
| `ind_cno_fin_ult1`           | Payroll Account                                                             |
| `ind_ctju_fin_ult1`          | Junior Account                                                              |
| `ind_ctma_fin_ult1`          | Más particular Account                                                      |
| `ind_ctop_fin_ult1`          | Particular Account                                                          |
| `ind_ctpp_fin_ult1`          | Particular Plus Account                                                     |
| `ind_deco_fin_ult1`          | Short-term deposits                                                         |
| `ind_deme_fin_ult1`          | Medium-term deposits                                                         |
| `ind_dela_fin_ult1`          | Long-term deposits                                                          |
| `ind_ecue_fin_ult1`          | E-account                                                                   |
| `ind_fond_fin_ult1`          | Funds                                                                       |
| `ind_hip_fin_ult1`           | Mortgage                                                                    |
| `ind_plan_fin_ult1`          | Pensions                                                                    |
| `ind_pres_fin_ult1`          | Loans                                                                       |
| `ind_reca_fin_ult1`          | Taxes                                                                       |
| `ind_tjcr_fin_ult1`          | Credit Card                                                                 |
| `ind_valo_fin_ult1`          | Securities                                                                   |
| `ind_viv_fin_ult1`           | Home Account                                                                |
| `ind_nomina_ult1`            | Payroll                                                                     |
| `ind_nom_pens_ult1`          | Pensions                                                                    |
| `ind_recibo_ult1`            | Direct Debit                                                                |



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

### **Performance**
- **Validation MAP@7 Score:** 0.04
  - Custom function used to calculate MAP@7, aligning with Kaggle competition requirements.  
- **Observations:**  
  - The model's score is .01 higher than other submissions which means there could be an issue in the evaluation function or the way predictions are being calculated

---
