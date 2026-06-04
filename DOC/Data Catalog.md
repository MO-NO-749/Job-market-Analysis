# Data Catalog — 2023 Job Market Analysis

**Tool:** Microsoft Excel (Power Query + Power Pivot)  
**Source:** Single CSV — 32,672 job postings, 2023  

---

## Table 1: Source Table (Raw Data)

> Loaded directly from CSV. No transformations. 32,672 rows × 16 columns.

| Column | Data Type | Nullable | Description | Sample Values |
|---|---|---|---|---|
| `job_title_short` | Text | No | Standardized job title (10 categories) | Data Analyst, Data Engineer |
| `job_title` | Text | No | Full job title as posted | "Data Engineer - MA" |
| `job_location` | Text | Yes (355 missing) | City and state | "Mesa, AZ" |
| `job_via` | Text | Yes (10 missing) | Platform where job was posted | via LinkedIn, via Indeed |
| `job_schedule_type` | Text | Yes (141 missing) | Employment type | Full-time, Contractor |
| `job_work_from_home` | Boolean | No | Whether the role is remote | TRUE / FALSE |
| `search_location` | Text | No | Location used in the job search | "Texas, United States" |
| `job_posted_date` | DateTime | No | When the job was posted | 2023-04-24 09:51:15 |
| `job_no_degree_mention` | Boolean | No | Degree not mentioned as requirement | TRUE / FALSE |
| `job_health_insurance` | Boolean | No | Health insurance offered | TRUE / FALSE |
| `job_country` | Text | No | Country of the posting | United States, France |
| `salary_rate` | Text | No | Pay period | year / hour |
| `salary_year_avg` | Decimal | Yes (10,636 missing — 32.5%) | Average annual salary in USD | 128050, 140000 |
| `salary_hour_avg` | Decimal | Yes (22,036 missing — 67.5%) | Average hourly rate in USD | 39.79, 61.15 |
| `company_name` | Text | No | Hiring company | Cox Communications |
| `job_skills` | Text (Array) | Yes (3,187 missing) | Skills listed as array string | ['sql', 'python', 'aws'] |

> ⚠️ Salary data is missing for a large share of records. All salary measures should be interpreted with that limitation in mind.

---

## Table 2: Job_Decp (Transformed — Power Query)

> Cleaned version of Source Table. Skills column retained. Index added. Date normalized.

| Column | Data Type | Description |
|---|---|---|
| `Index` | Integer | Row identifier added in Power Query |
| `job_title_short` | Text | Standardized title |
| `job_title` | Text | Full title |
| `job_location` | Text | Job location |
| `job_via` | Text | Posting platform |
| `job_schedule_type` | Text | Employment type |
| `job_work_from_home` | Boolean | Remote flag |
| `search_location` | Text | Search origin |
| `job_posted_date` | Date | Date extracted from datetime |
| `Job_Posted_Date` | Date | Used as join key to Calendar table |
| `job_no_degree_mention` | Boolean | Degree requirement flag |
| `job_health_insurance` | Boolean | Benefits flag |
| `job_country` | Text | Country |
| `salary_rate` | Text | Pay period |
| `salary_year_avg` | Decimal | Annual salary |
| `company_name` | Text | Hiring company |

---

## Table 3: Job_Skills (Exploded Skills)

> Skills array from Source Table split into one row per skill per job using Power Query.

| Column | Data Type | Description |
|---|---|---|
| `Index` | Integer | Links back to Job_Decp row |
| `Value` | Text | Individual skill (e.g. sql, python) |

**Join:** Job_Skills[Index] → Job_Decp[Index] (many skills per job) 

---

## Table 4: Job_Merged (Combined — used for DAX measures)

> Job_Decp and Job_Skills joined together. One row per job-skill pair. DAX measures are defined here.

| Column / Measure | Type | Description |
|---|---|---|
| `Index` | Integer | Row identifier |
| `job_title_short` | Text | Job title |
| `job_location` | Text | Location |
| `job_via` | Text | Platform |
| `job_schedule_type` | Text | Schedule |
| `job_work_from_home` | Boolean | Remote flag |
| `search_location` | Text | Search location |
| `job_posted_date` | Date | Posting date |
| `job_no_degree_mention` | Boolean | Degree flag |
| `salary_year_avg` | Decimal | Annual salary |
| `salary_rate` | Text | Rate type |
| `job_health_insurance` | Boolean | Insurance flag |
| `job_country` | Text | Country |
| `company_name` | Text | Company |
| `Job_Skills.Value` | Text | Skill name |
| `skillsperjob` | Measure | Avg skills per job posting |
| `Median Salary` | Measure | Median salary (all countries) |
| `Job_by_skills` | Measure | Job count per skill |
| `Median Salary of USA` | Measure | Median salary — USA only |
| `Median Salary Except USA` | Measure | Median salary — non-USA |

---

## Table 5: Calendar

> Date dimension table. Covers all dates in 2023.

| Column | Data Type | Description |
|---|---|---|
| `Date` | Date | Full date |
| `Year` | Integer | Year |
| `Month Number` | Integer | Month as number (1–12) |
| `Month` | Text | Month name |
| `MMM-YYYY` | Text | e.g. Jan-2023 |
| `Day Of Week Number` | Integer | Day number |
| `Day Of Week` | Text | Day name |
| `Start of Month` | Date | First day of month |
| `Reporting Period` | Text | Aggregated label |
| `Date Hierarchy` | Hierarchy | Year → Month → Date |

**Join:** Calendar[Date] → Job_Decp[Job_Posted_Date] (one date to many jobs)

---

## Data Quality Summary

| Issue | Column | Count | Note |
|---|---|---|---|
| Missing annual salary | salary_year_avg | 10,636 (32.5%) | Largest limitation — salary insights cover ~67% of data |
| Missing hourly salary | salary_hour_avg | 22,036 (67.5%) | Hourly analysis not included in dashboards Converted to salary_year_avg |
| Missing skills | job_skills | 3,187 | Excluded from skill counts |
| Missing location | job_location | 355 | search_location used where needed |
| Missing platform | job_via | 10 | Excluded from platform rankings |
| Missing schedule type | job_schedule_type | 141 | Shown as blank in filter |
