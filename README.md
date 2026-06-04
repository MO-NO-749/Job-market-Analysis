# 📊 2023 Job Market Analysis — Excel Project

> An Excel analytics project exploring 32,672 global job postings from 2023, built using Power Query for data transformation and Power Pivot for relational modelling — delivering two interactive dashboards on salary benchmarks, in-demand skills, and hiring trends.



## 🗂️ Project Structure

```
└── .xlsx/
    ├── Source Table          # Raw dataset (32,672 rows, 16 columns)
    ├── Sheet1                # Power Query cleaned & transformed data
    ├── Pivot_Table           # Aggregation layer feeding dashboard visuals
    ├── DashBoard1            # Job market overview dashboard
    ├── DashBoard2            # Salary & forecast dashboard
└── Docs/
    ├── Data Catalog
    ├── Data Model
    └── DAX_Measures
```
## Data Model - 
    Used a normal date and main table connection first but marged Two  tables for quick load. Skills montioned in for a job id 
---
![Data Model](DOC/Data%20Model%20.png)
## 🏗️ Data Transformation Approach

The project uses a 3-step flow within Excel:

| Step | Sheet | What Happens |
|---|---|---|
| **Raw** | Source Table | CSV data loaded as-is — no changes |
| **Transformed** | Power Query | Nulls handled, skills exploded into rows, date normalized |
| **Aggregated** | Pivot_Table + Dashboards | DAX measures applied, visuals built |

> Note: This is not a formal Medallion Architecture — it's a structured transformation flow within a single Excel workbook.

---

## 📖 Project Overview

| Item | Detail |
|---|---|
| **Total Records** | 32,672 job postings |
| **Time Period** | 2023 (Jan – Dec) |
| **Geographies** | 60+ countries |
| **Job Titles** | 10 standardized categories |
| **Top Platform** | LinkedIn (6,395 postings) |
| **Top Company** | Upwork (729 postings) |
| **Most Listed Skill** | SQL (18,500 mentions) |

---

## 📊 Dashboards

### Dashboard  — Job Market Overview
- Top 10 Companies Hiring
- Top 10 Job Posting Platforms
- Top 10 Skills vs Average Salary
- Top 10 Skills Count
- Jobs by Country (Map — bubble size = job count)
- Job Posts by Day of Week
- Salary by Skill (Median / USA / Non-USA)
- Slicers: Country, Job Title, Date

### Dashboard 1 — Salary & Forecast
- Salary by Job Role (3-line: Median, USA, Non-USA)
- Median Salary vs Avg Skills Required (scatter plot)
- Monthly Salary Trend + Polynomial Forecast (R²=0.71)
- Monthly Job Count Trend + Polynomial Forecast (R²=0.77)
- Slicers: Country, Job Title

---

## 🔧 Tools Used

- **Microsoft Excel** — Pivot Tables, Charts, Dashboards, Slicers
- **Power Query (M)** — Data cleaning, skill column explosion, date transformation
- **Power Pivot (DAX)** — Relational model, calculated measures

---

## 📋 Data Source

Single CSV file containing 2023 job postings scraped from public job boards.  
Skills were stored as array strings per row and exploded into individual rows during transformation.

---


## 🚀 Key Findings

1. Senior Data Engineer has the highest median salary (~$155K)
2. SQL is the most listed skill across all roles (18,500 mentions)
3. LinkedIn accounts for nearly 3× more postings than the next platform
4. Tuesday is the most active posting day (5,851 posts)
5. USA median salaries are 5–15% higher than global medians across roles
6. Job count shows a declining trend from mid-2023 peak through December

---

## 📁 How to Use

1. Open the `.xlsx` file in Excel 2019 or Microsoft 365
2. Go to **Job Market Overview** for market overview
3. Go to **Salary & Forecast** for salary and forecast analysis
4. Use slicers to filter by Country, Job Title, or Date
5. Do not edit the Source Table sheet directly

---
For more details, refer to [Documentation](DOC/)

## 🛡️ License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and share this project with proper attribution.

## 🌟 About Me

Hi there! I'm **Monojit Samanta**. I’m an B.com graduate want to excel in my professional life with data in front and finance as domain.

Let's stay in touch! Feel free to connect with me on the following platform:

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/monojit-samanta-720889383)

