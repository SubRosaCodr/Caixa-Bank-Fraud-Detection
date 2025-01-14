# Caixa-Bank-Fraud-Detection
This project focuses on analyzing customer data to evaluate risk levels using financial and behavioral metrics. The dataset includes key information such as income, debt, credit scores, and fraud history. Based on this analysis, customers are categorized into three distinct risk groups: High Risk, Medium Risk, and Low Risk.


### **Project Overview**

This project leverages SQL and BigQuery to analyze customer data, providing insights into risk levels based on financial and behavioral metrics. The key objectives include:

1. **Data Cleaning and Transformation**  
   - Ensured data fields are standardized and prepared for analysis.  

2. **Risk Categorization**  
   - Introduced a calculated `risk_category` column based on predefined criteria.  

3. **Dataset Enhancement**  
   - Added a `has_fraud_history` column to flag customers with a history of fraudulent behavior.  

4. **Final Dataset Publication**  
   - Generated a clean, enriched dataset for reporting and further analysis.  

---

### **Dataset**

The dataset comprises the following fields:

| **Column Name**       | **Type**    | **Description**                                              |
|------------------------|-------------|--------------------------------------------------------------|
| `customer_id`          | INTEGER     | Unique identifier for each customer.                        |
| `current_age`          | INTEGER     | Current age of the customer.                                |
| `yearly_income`        | INTEGER     | Annual income of the customer.                              |
| `total_debt`           | INTEGER     | Total debt of the customer.                                 |
| `credit_score`         | INTEGER     | Credit score of the customer.                               |
| `per_capita_income`    | INTEGER     | Per capita income.                                          |
| `retirement_age`       | INTEGER     | Expected retirement age.                                    |
| `gender`               | STRING      | Gender of the customer.                                     |
| `num_credit_cards`     | INTEGER     | Number of credit cards owned.                               |
| `fraud_count`          | INTEGER     | Number of recorded fraud incidents.                         |
| `has_fraud_history`    | INTEGER     | Indicates fraud history (`1` for yes, `0` for no).          |
| `risk_category`        | STRING      | Categorized risk level of the customer.                     |

---

### **⚙ Risk Categorization Logic**

The `risk_category` is determined based on the following rules:  

- **High Risk**:  
  - `fraud_count` > 20  
  - `credit_score` < 600 **AND** `total_debt` > 50,000  

- **Medium Risk**:  
  - `fraud_count` between 1 and 20  
  - `credit_score` between 600 and 750 **AND** `total_debt` between 20,000 and 50,000  

- **Low Risk**:  
  - `fraud_count` = 0 **AND** `credit_score` >= 750  

- **Default**:  
  - If none of the conditions are met, customers are categorized as **Low Risk**.  

---

### ** Steps to Reproduce**

1. **Data Cleaning**  
   - Load the raw data into BigQuery.  
   - Validate data types and handle duplicates or null values.  

2. **Add `risk_category` Column**  
   Use the following SQL query to classify customers into risk levels:  

   ```sql
   CREATE OR REPLACE TABLE `project.dataset.generateddata_with_risk` AS
   SELECT *,
       CASE
           WHEN fraud_count > 20 THEN 'High Risk'
           WHEN credit_score < 600 AND total_debt > 50000 THEN 'High Risk'
           WHEN fraud_count BETWEEN 1 AND 20 THEN 'Medium Risk'
           WHEN credit_score BETWEEN 600 AND 750 AND total_debt BETWEEN 20000 AND 50000 THEN 'Medium Risk'
           WHEN fraud_count = 0 AND credit_score >= 750 THEN 'Low Risk'
           ELSE 'Low Risk'
       END AS risk_category
   FROM `project.dataset.generateddata`;
   ```

3. **Add `has_fraud_history` Column**  
   Create a column to indicate fraud history:  

   ```sql
   CREATE OR REPLACE TABLE `project.dataset.generateddata_with_fraud_history` AS
   SELECT *,
       CASE
           WHEN fraud_count > 0 THEN 1
           ELSE 0
       END AS has_fraud_history
   FROM `project.dataset.generateddata_with_risk`;
   ```

4. **Publish Final Table**  
   Export the final table with all enriched columns for visualization or further reporting.

   ![image](https://github.com/user-attachments/assets/32716df2-94a5-4043-8c1d-603dc792b542)



---

### ** Tools & Technologies**

- **Google BigQuery**: For SQL-based data analysis and transformations.  
- **Python**: Used for machine learning integration and data preprocessing.  
- **Power BI**: For interactive data visualization and reporting.  
- **GitHub**: Version control and project documentation.  

---

### ** File Structure**

```
.
├── 📁 Database/           # Dataset storage and updates
├── 📁 app/                # Scripts and code for data transformations
├── README.md              # Project documentation
├── clf.pkl                # Machine learning model file
├── Dashboard.png          # Sample data visualization
├── 📁 SQL_Scripts/        # SQL queries for data processing
└── 📁 Outputs/            # Final results and reports
```

---

### ** Visualization**  
Data visualizations are included in the project to highlight key insights and fraud patterns. View sample visualizations in the provided `Dashboard.png` file.

---

### ** How to Run**

1. **Clone the Repository**:  
   ```bash
   git clone https://github.com/SubRosaCodr/Caixa-Bank-Fraud-Detection.git
   ```
2. **Navigate to the Project Directory**:  
   ```bash
   cd Caixa-Bank-Fraud-Detection
   ```
3. **Execute SQL Scripts**:  
   Run the provided SQL queries in BigQuery to process the data.  
4. **Analyze Results**:  
   Used to visualize the outcomes.

---

### ** License**  
This project is licensed under the **MIT License**. See the `LICENSE` file for further details.

---

### ** Project Maintainer**  
** Donald Behrendt**  

Special thanks to the incredible contributors:  
** Amir, Kai, and Valery**

---

### ** Dataset Source**  
The dataset used in this project is sourced from Kaggle’s **Transactions Fraud Datasets**.


