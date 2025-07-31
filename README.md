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
- **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specialization within data analytics.

### 2. Top-paying Data Analyst job_skills
To identify the Top-paying Data Analyst job skills, I filtered data analyst positions by average yearly salary and job skills, 

```sql
WITH top_paying_jobs AS (
SELECT 
    job_id,
    job_title,
    salary_year_avg,
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
)

SELECT 
    top_paying_jobs.*,
    skills
FROM
    top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    top_paying_jobs.salary_year_avg DESC;

```
Here's the breakdown of the top-paying data analyst job skills in 2023:
- SQL, Python, and Tableau are the most frequently demanded skills.

- AWS and Power BI are associated with the highest average salaries.

- Pandas and Excel, while common, are also linked to high-paying 
  roles—likely due to their use in advanced analytics and business 
  reporting.

- Strong data stack skills (e.g., SQL, Snowflake, Python, Pandas) and 
  data visualization tools (Tableau, Power BI) dominate the landscape.

### 3. Top-Demanded skills for Data Analyst jobs
To identify the top-demanded skills for Data Analyst jobs, I filtered data analyst positions by job skills, and ranked them by skills based on skill_count

```sql
SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_skill_count
FROM
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'India'
GROUP BY
    skills
ORDER BY
    demand_skill_count DESC
LIMIT 5

```
Here's the breakdown of the top-demanded skills for Data Analyst jobs in India:

| Skill    | Job Postings |
| -------- | ------------ |
| SQL      | 1016         |
| Excel    | 717          |
| Python   | 687          |
| Tableau  | 545          |
| Power BI | 402          |

![Top-15 Demanded skills for Data Analyst in India](assets\top 15 most demanded skills.png)
*Bar graph visualizing the salary for the top 10 salaries  for data analysts; ChatGPT generated this graph from my sql query results*

### 4. Skills Based on Salary

Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM
    job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' AND
    salary_year_avg IS NOT NULL AND
    job_work_from_home = TRUE
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```

Here's a breakdown of Insights:
- Big Data & ML Tools Pay the Most: Skills like PySpark, Databricks, Scikit-learn, and DataRobot are linked to salaries above $150K, reflecting demand for analysts with machine learning and data engineering expertise.
- Cloud & DevOps Knowledge Boosts Value: Tools such as GCP, Kubernetes, Airflow, and GitLab show that analysts who can work with automated pipelines and scalable cloud environments are highly paid.
- Coding Versatility Matters: High salaries for Pandas, Numpy, PostgreSQL, and even Swift/Golang suggest that programming fluency beyond basic Python is a strong differentiator in analytics roles.

| Skill         | Average Salary |
|---------------|----------------|
| pyspark       |         208,172|
| bitbucket     |         189,155|
| couchbase     |         160,515|
| watson        |         160,515|
| datarobot     |         155,486|
| gitlab        |         154,500|
| swift         |         153,750|
| jupyter       |         152,777|
| pandas        |         151,821|
|elasticsearch  |         145,000|

*Table of the average salary or the top 10 paying skills for data analysts*

### 5. Most Optimal Skills ro Learn for Data Analysts

Combining insights from demand and salary data. this query aimed to piinpoint skills that are both in high demand and have high salaries offering strategic focus for skill development.

```sql

WITH skills_demand AS (
    SELECT
        skills_dim.skill_id,
        skills_dim.skills,
        COUNT(skills_job_dim.job_id) AS demand_skill_count
    FROM
        job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' AND
        salary_year_avg IS NOT NULL AND
        job_work_from_home = True
    GROUP BY
        skills_dim.skill_id,
        skills_dim.skills
), average_salary AS (
    SELECT
        skills_job_dim.skill_id,
        ROUND(AVG(salary_year_avg), 0) AS avg_salary
    FROM
        job_postings_fact
    INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
    INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
    WHERE
        job_title_short = 'Data Analyst' AND
        salary_year_avg IS NOT NULL AND
        job_work_from_home = True
    GROUP BY
        skills_job_dim.skill_id
)

SELECT
    skills_demand.skill_id,
    skills_demand.skills,
    skills_demand.demand_skill_count,
    average_salary.avg_salary
FROM
    skills_demand
INNER JOIN average_salary ON skills_demand.skill_id = average_salary.skill_id
WHERE
    demand_skill_count > 10
ORDER BY
    avg_salary DESC,
    demand_skill_count DESC
LIMIT 25;
```



| Skill ID | Skill       | Demand Count | Average Salary ($) |
|----------|-------------|---------------|---------------------|
| 8        | go          | 27            | 115,320             |
| 234      | confluence  | 11            | 114,210             |
| 97       | hadoop      | 22            | 113,193             |
| 80       | snowflake   | 37            | 112,948             |
| 74       | azure       | 34            | 111,225             |
| 77       | bigquery    | 13            | 109,654             |
| 76       | aws         | 32            | 108,317             |
| 4        | java        | 17            | 106,906             |
| 194      | ssis        | 12            | 106,683             |
| 233      | jira        | 20            | 104,918             |

*Table of the most optimal skills for data analyst sorted by salary*

### 🔍 Breakdown of the Most Optimal Skills for Data Analysts in 2023:

- **High-Demand Programming Languages:** Python and R stand out for their high demand, with demand counts of 236 and 148 respectively. Despite their high demand, their average salaries are around $101,397 for Python and $100,499 for R, indicating that proficiency in these languages is highly valued but also widely available.

- **Cloud Tools and Technologies:** Skills in specialized technologies such as Snowflake, Azure, AWS, and BigQuery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big data technologies in data analysis.

- **Business Intelligence and Visualization Tools:** Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, highlight the critical role of data visualization and business intelligence in deriving actionable insights from data.

- **Database Technologies:** The demand for skills in traditional and NoSQL databases (Oracle, SQL Server, NoSQL) with average salaries ranging from $97,786 to $104,534, reflects the enduring need for data storage, retrieval, and management expertise.

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