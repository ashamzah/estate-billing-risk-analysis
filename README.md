# Revenue Leakage & Billing Control Tower Analysis

## SML Township Management 2021–2024

![Key Visual](images/key_visual.png)

This project analyzes township estate billing data to identify revenue leakage, collection risks, data quality issues, utility billing anomalies, and ERP logical errors. The analysis supports a **billing control tower** approach to improve cashflow visibility, collection effectiveness, billing accuracy, and operational control.

---

## Project Overview

Township management revenue can be affected by unpaid IPL invoices, overdue receivables, vacant units, incomplete owner contact data, inconsistent payment method records, utility billing outliers, and ERP system anomalies.

This project focuses on identifying the most critical billing and collection risks, then translating the findings into actionable recommendations for **risk-based collection**, **billing validation**, and **data quality improvement**.

---

## Repository Structure

```text
.
├── README.md
├── SML_Estate_Billing_Control_Tower_Analysis.ipynb
│
├── images/
│   ├── dashboard_preview.png
│   ├── executive_summary.png
│   └── key_visual.png
│
├── presentation/
│   └── SML_Estate_Billing_Control_Tower_Analysis.pdf
│
└── requirements.txt
```

### File Description

| File / Folder | Description |
|---|---|
| `README.md` | Project documentation, business context, key findings, recommendations, and resource links |
| `SML_Estate_Billing_Control_Tower_Analysis.ipynb` | Main notebook containing data cleaning, analysis, visualization, and insight generation |
| `images/dashboard_preview.png` | Dashboard preview image for portfolio documentation |
| `images/executive_summary.png` | Executive summary visual used to summarize priority areas |
| `images/key_visual.png` | Main visual representation of conclusion, risks, and improvement focus |
| `presentation/SML_Estate_Billing_Control_Tower_Analysis.pdf` | Final presentation deck in PDF format |
| `requirements.txt` | Python dependencies used in the analysis |

---

## Business Objectives

The main objectives of this project are:

- Identify revenue leakage from unpaid and overdue IPL billings.
- Analyze aging schedule to determine the highest-risk outstanding receivables.
- Evaluate vacant property risk and its relationship with billing arrears.
- Assess collection data quality through missing contact number analysis.
- Standardize payment method categories to support finance reconciliation.
- Detect extreme water usage outliers that may indicate utility billing issues.
- Identify ERP logical errors, especially paid invoices recorded before handover date.
- Provide actionable recommendations for risk-based collection and billing control.

---

## Dataset Overview

The analysis uses estate billing data from BigQuery covering the period **2021–2024** with approximately **300,000 records**.

Main source tables:

| Table | Description |
|---|---|
| `clusters` | Township and cluster information |
| `units` | Unit ownership, contact number, vacant status, and handover date |
| `IPL billings` | Invoice, billing amount, water usage, payment status, payment method, and payment date |

Clean data output:

| Item | Value |
|---|---|
| Dataset | `SML_clean` |
| Table | `SML_data` |
| Analysis Tool | Google Colab / Python |
| Data Warehouse | BigQuery |
| Visualization Tool | Looker Studio |

> Note: The original dataset is not included in this repository due to confidentiality. The notebook contains the analysis workflow and methodology.

---

## Methodology

The project workflow consists of five main steps:

1. **Data Source**  
   Import raw tables from BigQuery: clusters, units, and IPL billings.

2. **Data Integration & Cleaning**  
   Join datasets, format date columns, handle missing values, check duplicates, standardize payment method, and create anomaly flags.

3. **Data Analysis**  
   Perform exploratory data analysis, revenue leakage analysis, aging analysis, vacant risk analysis, collection data quality analysis, water outlier detection, and Chi-Square Test.

4. **Clean Data Export**  
   Export the cleaned dataset back to BigQuery as `SML_clean.SML_data`.

5. **Data Visualization**  
   Build dashboard pages in Looker Studio for executive overview, collection & aging, vacant property risk, water & utility, and data quality monitoring.

---

## Dashboard Preview

![Dashboard Preview](images/dashboard_preview.png)

---

## Executive Summary Visual

![Executive Summary](images/executive_summary.png)

---

## Key Analysis Areas

### 1. Revenue Leakage

Revenue leakage is measured through the ratio of unpaid and overdue billing amount. The tunggakan rate is relatively similar across townships, ranging from approximately **28.5% to 29.9%**. This indicates that collection risk is systemic across townships, not isolated to only one area.

### 2. Aging Schedule

Aging analysis shows that the largest outstanding amount is concentrated in invoices aged more than 6 months. The outstanding amount in the `> 6 months` bucket reaches approximately **Rp82.51 billion**. BSD City, Kota Wisata, and Grand Wisata contribute around **82.7%** of total delayed revenue over 6 months, making them the main priority for collection recovery.

### 3. Vacant Property Risk

Vacant units show a much higher tunggakan rate compared to occupied units. The vacant unit tunggakan rate reaches **59.7%**, almost 3 times higher than occupied units. Chi-Square Test shows a significant relationship between occupancy status and tunggakan status with **p < 0.01**.

### 4. Collection Data Quality

Missing contact number is not statistically significant as a direct cause of tunggakan, with **p > 0.05**. However, it remains an operational barrier because it limits the collection team's ability to follow up with unit owners. Missing contact is relatively evenly distributed across townships, approximately **19%–21%**.

### 5. Payment Method Cleansing

Payment method records were previously inconsistent, with multiple variations for the same payment channel. Cleaning reduced payment method categories from **10 categories to 5 standardized categories**, improving the consistency of channel analysis and finance reconciliation. However, payment method values marked as `None` still require attention.

### 6. Water Usage Outlier

Extreme water usage outliers are relatively low, less than **1%**, but they still need validation because they may lead to inaccurate utility billing. The highest outlier rate is found in **BSD City - Commercial Ruko**, at approximately **0.65%**.

### 7. ERP Logical Error

ERP logical error analysis identifies invoices with `Paid` status where `payment_date` occurs before `handover_date`. This indicates a potential billing logic issue, data migration issue, or incorrect date mapping. The analysis found **6,291 anomaly invoices** with a total value of approximately **Rp8.35 billion**. Around **82.9%** of anomaly invoices are concentrated in BSD City, Kota Wisata, and Grand Wisata.

---

## Key Findings

| Area | Key Finding | Business Impact |
|---|---|---|
| Revenue Leakage | Tunggakan rate is relatively similar across townships | Collection issue is systemic |
| Aging Schedule | Outstanding >6 months reaches Rp82.51 billion | High recovery risk and delayed cashflow |
| Vacant Property | Vacant tunggakan rate reaches 59.7% | Vacant units require special collection strategy |
| Collection Data Quality | Missing contact is not significant statistically but affects follow-up | Slower collection process |
| Payment Method | Categories reduced from 10 to 5 | Better reconciliation and channel analysis |
| Water Usage Outlier | Outlier rate is low but still needs validation | Prevents utility billing errors |
| ERP Logical Error | 6,291 invoices paid before handover, worth Rp8.35 billion | Risk of audit finding and incorrect revenue recording |

---

## Recommendations

- Apply **risk-based collection** using aging, outstanding amount, vacant status, and contact availability.
- Prioritize collection on **BSD City, Kota Wisata, and Grand Wisata**, especially for invoices aged more than 6 months.
- Create a dedicated collection segment for **vacant units**, especially vacant + unpaid/overdue units.
- Perform regular contact number cleansing, focusing on units with **Missing Contact + Unpaid/Overdue** status.
- Standardize payment method records and monitor `None` payment method values for finance reconciliation.
- Validate extreme water usage before invoice issuance to reduce utility billing errors.
- Implement ERP validation rules so that `payment_date` cannot be earlier than `handover_date` for active invoices.
- Build continuous billing control monitoring through Looker Studio dashboards.

---

## Dashboard & Presentation

| Resource | Link |
|---|---|
| Looker Studio Dashboard | [Open Dashboard](https://datastudio.google.com/reporting/00e03556-32a8-4e33-b38c-46b9e3feef00) |
| PowerPoint Presentation | [Open Presentation Folder](https://drive.google.com/drive/folders/1OQqQ37DDp1bikiGwiu11A8IK3GgJwfd3?usp=sharing) |
| Personal Blog | [arissandohamzah.hantulaut.web.id](https://arissandohamzah.hantulaut.web.id/) |
| LinkedIn | [Aris Sando Hamzah]([https://arissandohamzah.hantulaut.web.id/](https://www.linkedin.com/in/aris-sando-hamzah-5391501ab/)) |

---

## Tools & Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- SciPy
- Google Colab
- BigQuery
- Looker Studio
- GitHub

---

## How to Run

1. Clone this repository.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Open the notebook:

```bash
jupyter notebook SML_Estate_Billing_Control_Tower_Analysis.ipynb
```

4. Run the notebook cells sequentially.

> BigQuery credentials and access are required to fully reproduce the analysis.

---

## Author

**Aris Sando Hamzah**  
Data Analyst Portfolio Project
