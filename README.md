# 📊 SQL Project: Job Postings Data Analysis

## 🧩 Introduction
This project explores a real-world job postings dataset using advanced SQL techniques. The objective is to derive meaningful insights about in-demand skills, salary trends, work preferences, and hiring locations — specifically for **Data Analyst** roles in India.

By leveraging structured queries and common table expressions (CTEs), this project demonstrates how SQL can be used to extract insights that support career research, HR strategy, and market understanding.

SQL queries? Check them out here: [sql_project folder](/sql_load/)

---

## 🕰️ Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, sttreamlining others work to find optimal jobs.

### The questions I wanted to answer through my SQL queriies were:
1. What are the top-paying data analyst jobs?
2. What skills are required for these top-paying jobs?
3. What skills are most in demand for data analysts?
4. Which skills are associated with higher salaries?
5. What are the most optimal skills to learn?

---

## 🛠 Tools I Used
For my deep dive into the data analyst job market, I harnessed  the power of several key tools:

- **SQL**: The backbone of my analysis, allowing me to query the database and unearth critical insiights.
- **PostgreSQL**: The chosen database management system, ideal for handling the job posting data.
- **Git & GitHub**: Essential for version control and sharing my SQL scripts and analysis, ensuring collaborate and project tracking.
---
## The Analysis
Each query for this project aimed at iinvestigatiing specifiic aspects of the data analyst job market.
Here's how I approached each question:

### 1. Top paying Data Analyst jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high payinng opportunities in the field. 
```sql
SELECT 
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_location = 'India' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10

```
Here's the breakdown of the top data analyst jobs in 2023:
- **Wide Salary Range:** Top 10 paying data analyst roles span from $190,000 to $222,000, indicating significant salary potential in the field.

- **Diverse Employees:** Companies like SmartAsset, Meta, and AT&T are amoung those offering high salaries, showing a broad intrest across different industries.
- **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specialization within data analytics,

## 📚 What I Learned
- How to **join multiple normalized tables** to extract meaningful results
- Usage of **Common Table Expressions (CTEs)** for breaking down complex queries
- Identifying **top-demand skills** using `COUNT` aggregation
- Calculating **average salaries** by skill and job criteria
- Filtering and grouping data with **real-world conditions** like location, job role, and remote work
- Debugging SQL issues like **ambiguous column references** and **grouping mismatches**

---

## ✅ Conclusion
This project strengthened my SQL skills and showed how data storytelling can emerge from raw job data. The insights can help:
- Job seekers identify valuable skills to learn
- Employers benchmark salaries
- Analysts build dashboards or reports from structured data

The final queries simulate a mini business intelligence layer powered by SQL — turning data into decisions.

---