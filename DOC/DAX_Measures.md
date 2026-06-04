# DAX Measures — 2023 Job Market Analysis

> These measures are written in DAX and defined in Excel's Power Pivot model on the Job_Merged table.
> Note: Excel Power Pivot uses DAX but has less functionality than Power BI — some DAX functions available in Power BI do not work here.

---

## Current Measures

### 1. Median Salary

```dax
Median Salary =
MEDIAN(Job_Merged[salary_year_avg])
```

**What it does:** Returns the median annual salary for job postings in the current filter context.  
**Important:** 32.5% of rows have no salary — MEDIAN silently excludes nulls. Results reflect only postings that reported salary.  
**Used in:** Salary by role chart, salary per skill breakdown, scatter plot.

---

### 2. Median Salary of USA

```dax
Median Salary of USA =
CALCULATE(
    MEDIAN(Job_Merged[salary_year_avg]),
    Job_Merged[job_country] = "United States"
)
```

**What it does:** Same as Median Salary but filtered to United States postings only.  
**Used in:** Salary comparison chart, salary per skill table.

---

### 3. Median Salary Except USA

```dax
Median Salary Except USA =
CALCULATE(
    MEDIAN(Job_Merged[salary_year_avg]),
    Job_Merged[job_country] <> "United States"
)
```

**What it does:** Median salary for all non-US postings.  
**Used in:** Salary comparison chart for global vs US benchmarking.

---

### 4. Skills Per Job

```dax
skillsperjob =
DIVIDE(
    COUNTROWS(Job_Merged),
    DISTINCTCOUNT(Job_Merged[Index])
)
```

**What it does:** Calculates how many skills are listed on average per job posting.  
**How it works:** Job_Merged has one row per job-skill pair — dividing total rows by unique job count gives the average skills per posting.  
**Used in:** Scatter plot (X = salary, Y = skills per job).

---

### 5. Job by Skills

```dax
Job_by_skills =
COUNTROWS(Job_Merged)
```

**What it does:** Counts job postings associated with each skill in the current filter context.  
**Why it works:** Because Job_Merged is one row per job-skill pair, counting rows in a skill filter context equals counting how many jobs list that skill.  
**Used in:** Top 10 Skills Count chart, Skills vs Salary combo chart.

---

## Suggested Additions (from Improvement Plan)

These don't exist yet but would add meaningful value:

```dax
-- Share of remote jobs
Job Remote =
DIVIDE(
    COUNTROWS(FILTER(Job_Merged, Job_Merged[job_work_from_home] = TRUE())),
    COUNTROWS(Job_Merged)
)

-- Share of jobs not requiring a degree
Job No Degree =
DIVIDE(
    COUNTROWS(FILTER(Job_Merged, Job_Merged[job_no_degree_mention] = TRUE())),
    COUNTROWS(Job_Merged)
)

-- Share of jobs offering health insurance
Job Health Insurance =
DIVIDE(
    COUNTROWS(FILTER(Job_Merged, Job_Merged[job_health_insurance] = TRUE())),
    COUNTROWS(Job_Merged)
)

-- Share of postings that include salary data (data quality metric)
Job With Salary =
DIVIDE(
    COUNTROWS(FILTER(Job_Merged, NOT(ISBLANK(Job_Merged[salary_year_avg])))),
    COUNTROWS(Job_Merged)
)
```
