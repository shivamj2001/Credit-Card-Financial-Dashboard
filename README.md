# 📊 Credit Card Financial Dashboard

This repository contains Power BI dashboards and data transformation scripts to analyze credit card customer behavior and financial transactions on a weekly and quarterly basis. The project combines raw CSV files, SQL preprocessing, and Power BI visualizations with DAX measures to generate actionable insights.


## 📊 Dashboard Overview

### 📁 1. Financial Dashboard - **Transaction Focused**

![Uploading image.png…]()

This dashboard includes:

- **Revenue Analysis:**
  - Quarterly Revenue & Transactions
  - Revenue by Card Type (Blue, Silver, Gold, Platinum)
  - Revenue by Expenditure Type (Food, Fuel, Travel, etc.)
  - Payment Usage Method: Swipe, Online, Chip
- **Financial Metrics:**
  - Annual Fees
  - Interest Earned

### 👤 2. Financial Dashboard - **Customer Focused**
This dashboard highlights:

- **Demographic Segmentation:**
  - Revenue by Age Group, Gender, Education Level, Marital Status
  - Top 5 States by Revenue
- **Credit Score Summary (CSS)**
- **Income Group Distribution**
- **Salary Group Analysis**

---

## 🧭 Interactive Features

- **Tree Map Filters:**  
  Tree maps are implemented to act as filters across multiple visuals. When a segment (e.g., card type or state) is selected in the tree map, the entire dashboard dynamically updates to reflect filtered insights.

---

## 📌 Tools & Technologies

- **Power BI** – Dashboard creation, DAX measures, filtering
- **SQL** – Initial data preparation and transformation
- **CSV Files** – Raw input for customer and transaction data

---

## 📁 Files Included

| File Name                            | Description                                      |
|-------------------------------------|--------------------------------------------------|
| `credit_card.csv`                   | Weekly transaction-level data                    |
| `customer.csv`                      | Weekly customer demographic data                 |
| `cc_add.csv`                        | Final week's additional transaction data         |
| `cust_add.csv`                      | Final week's additional customer data            |
| `SQL Query - Financial Dashboard Data.sql` | SQL script used for initial data prep      |
| `Financial Dashboard Transaction.pdf` | Dashboard focused on transaction KPIs        |
| `Financial Dashboard-Customer.pdf`    | Dashboard focused on customer demographics    |

---

## 🔄 Data Preparation & Cleaning Steps

### 1. 🧩 Dataset Merging
Weekly datasets were merged with the final week datasets using `UNION`:
```sql
SELECT * FROM credit_card
UNION
SELECT * FROM cc_add

SELECT * FROM customer
UNION
SELECT * FROM cust_add
```

### 2. 🧠 Feature Engineering (Power BI)

#### ✅ New Columns Added:
- **Age Group** (Categorized from `customer_age`)
  ```DAX
  AgeGroup = SWITCH(
      TRUE(),
      [customer_age] < 30, "20-30",
      [customer_age] >= 30 && [customer_age] < 40, "30-40",
      [customer_age] >= 40 && [customer_age] < 50, "40-50",
      [customer_age] >= 50 && [customer_age] < 60, "50-60",
      [customer_age] >= 60, "60+",
      "Unknown"
  )
  ```

- **Income Group** (Categorized from `income`)
  ```DAX
  IncomeGroup = SWITCH(
      TRUE(),
      [income] < 35000, "Low",
      [income] >= 35000 && [income] < 70000, "Med",
      [income] >= 70000, "High",
      "Unknown"
  )
  ```

- **Revenue** (Core metric combining multiple financial indicators)
  ```DAX
  Revenue = [annual_fees] + [total_trans_amt] + [interest_earned]
  ```

- **Week Number** (To track week-wise metrics)
  ```DAX
  week_num2 = WEEKNUM([week_start_date])
  ```

#### 📈 Time-based Revenue KPIs:
- **Current Week Revenue**
  ```DAX
  Current_Week_Revenue = CALCULATE(
      SUM([Revenue]),
      FILTER(
          ALL('cc_detail'),
          [week_num2] = MAX([week_num2])
      )
  )
  ```

- **Previous Week Revenue**
  ```DAX
  Previous_Week_Revenue = CALCULATE(
      SUM([Revenue]),
      FILTER(
          ALL('cc_detail'),
          [week_num2] = MAX([week_num2]) - 1
      )
  )
  ```

- **WoW Revenue Change (% Week over Week)**
  ```DAX
  WoW_Revenue = DIVIDE(
      [Current_Week_Revenue] - [Previous_Week_Revenue],
      [Previous_Week_Revenue]
  )
  ```

---

## 🧠 Insights Gained

- High-revenue customers are typically in the 30–50 age group and fall into the medium to high-income brackets.
- Platinum card users and online spenders generate significantly more revenue.
- Food and Travel remain the top transaction categories contributing to revenue.
- Certain states contribute disproportionately high revenue, aiding in geographic targeting.

---

## ✅ How to Use

1. Load all CSV files into Power BI.
2. Run the SQL script if prepping from database.
3. Create calculated columns and measures as shown above.
4. Build dashboards using stacked bars, pie charts, cards, and tree maps.
5. Publish and share insights interactively.

---

## 📬 Contact

For questions or collaboration, please reach out via GitHub or your professional contact point.

---

```  

Let me know if you'd like a lighter version of this for documentation or if you want this converted into a Word doc or PDF as well.
