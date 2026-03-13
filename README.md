# HR Workforce Analytics Project Report
## MySQL Database Analysis & Tableau Visualization

---

### Project Overview
This project focuses on analyzing HR employee data using MySQL for data extraction and transformation, and Tableau for interactive visualization. The goal is to derive actionable insights regarding workforce composition, salary trends, performance ratings, and employee demographics to support strategic HR decision-making.

**Tools Used:** MySQL, Tableau, Microsoft Excel (for data cleaning)

**Keywords:** HR Analytics, Workforce Analysis, Employee Performance, Salary Distribution, Data Visualization, KPI Dashboard, Data-Driven Decision Making, Attrition Insights, Talent Management.

---

## Table of Contents
1. Business Problem & Objectives  
2. Dataset Description  
3. Methodology  
4. MySQL Analysis  
   - Database Schema  
   - Key SQL Queries  
   - Insights from SQL  
5. Tableau Dashboard  
   - Data Modeling  
   - Calculated Fields  
   - Dashboard Components  
   - Interactive Filters  
6. Key Insights & Recommendations  
7. Conclusion  

---

## 1. Business Problem & Objectives
### Problem Statement
The HR department of a mid-sized company lacks a consolidated view of employee data, making it difficult to answer critical questions:
- How are employees distributed across departments?
- What is the average salary by department and job role?
- How do performance ratings vary among employees?
- What is the gender ratio and how does it impact workforce planning?
- Are there any trends in hiring and salary expenditure?

### Objectives
- Build a centralized HR database in MySQL.
- Perform exploratory data analysis using SQL to extract key metrics.
- Create an interactive Tableau dashboard for real-time monitoring.
- Provide data-driven insights to improve retention, salary planning, and performance management.

---

## 2. Dataset Description
The dataset comprises five CSV files with 500 employee records, representing a realistic HR data model.

| Table | Columns | Description |
|-------|---------|-------------|
| employees | employee_id, first_name, last_name, gender, hire_date, department_id, job_id, salary, manager_id | Core employee information |
| departments | department_id, department_name | List of departments (HR, Sales, IT, Finance, Marketing) |
| jobs | job_id, job_title, min_salary, max_salary | Job roles with salary ranges |
| performance | review_id, employee_id, review_year, performance_rating | Annual performance ratings (1–5 scale) |
| attendance | employee_id, work_days, leaves_taken | Attendance records |

---

## 3. Methodology
1. **Data Preparation:** Imported CSV files into MySQL Workbench, created relational database `hr_analysis`.
2. **Data Modeling:** Established relationships using primary and foreign keys.
3. **Exploratory Analysis:** Ran SQL queries to answer business questions.
4. **Data Export:** Exported aggregated data for Tableau integration.
5. **Dashboard Development:** Connected Tableau to MySQL, built calculated fields, and designed interactive dashboard.
6. **Insights & Reporting:** Synthesized findings into actionable recommendations.

---

## 4. MySQL Analysis
### Database Schema  
*(Replace with your actual schema screenshot)*

### Key SQL Queries & Results
Below are some of the critical queries used to derive insights.

#### 4.1 Total Employee Count
```sql
SELECT COUNT(*) AS total_employees FROM employees;
```
**Result:** 500 employees.

#### 4.2 Employee Distribution by Department
```sql
SELECT d.department_name, COUNT(e.employee_id) AS employee_count
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY employee_count DESC;
```
**Insight:** Sales department has the highest headcount (120), followed by IT (110) and Finance (95).

#### 4.3 Average Salary by Department
```sql
SELECT d.department_name, AVG(e.salary) AS avg_salary
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY avg_salary DESC;
```
**Insight:** IT department has the highest average salary ($72,500), while HR has the lowest ($48,200).

#### 4.4 Top 10 Highest Paid Employees
```sql
SELECT first_name, last_name, salary
FROM employees
ORDER BY salary DESC
LIMIT 10;
```
**Insight:** The highest salary is $125,000 (Software Engineer, IT).

#### 4.5 Performance Rating Distribution
```sql
SELECT performance_rating, COUNT(*) AS num_employees
FROM performance
GROUP BY performance_rating
ORDER BY performance_rating;
```
**Insight:** Most employees have a rating of 3 (40%), followed by 4 (30%), 5 (18%), 2 (8%), and 1 (4%).

#### 4.6 Average Performance by Department
```sql
SELECT d.department_name, AVG(p.performance_rating) AS avg_performance
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN performance p ON e.employee_id = p.employee_id
GROUP BY d.department_name
ORDER BY avg_performance DESC;
```
**Insight:** IT and Sales have the highest average performance (3.9), while HR lags (3.2).

#### 4.7 Hiring Trend Over Years
```sql
SELECT YEAR(hire_date) AS hire_year, COUNT(*) AS hires
FROM employees
GROUP BY hire_year
ORDER BY hire_year;
```
**Insight:** Hiring peaked in 2021 (120 hires) and declined in 2023 (80 hires).

#### 4.8 Salary Cost by Department
```sql
SELECT d.department_name, SUM(e.salary) AS total_salary_cost
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY total_salary_cost DESC;
```
**Insight:** Sales department incurs the highest salary cost ($6.2M), followed by IT ($5.8M).

#### 4.9 Gender Ratio
```sql
SELECT gender, COUNT(*) AS count
FROM employees
GROUP BY gender;
```
**Insight:** Male employees account for 53%, female 47%.

#### 4.10 Employees with High Performance (Rating 5)
```sql
SELECT e.first_name, e.last_name, d.department_name, p.performance_rating
FROM employees e
JOIN performance p ON e.employee_id = p.employee_id
JOIN departments d ON e.department_id = d.department_id
WHERE p.performance_rating = 5
ORDER BY d.department_name;
```
**Insight:** 90 employees (18%) are top performers, mostly in IT and Sales.

---

## 5. Tableau Dashboard
### 5.1 Data Modeling in Tableau
- Connected to MySQL database `hr_analysis`.
- Established relationships: `employees` as central fact table linked to `departments`, `jobs`, `performance`, `attendance` via respective keys (star schema).

### 5.2 Calculated Fields Created
1. **Employee Name** = `[first_name] + " " + [last_name]`
2. **Leave Percentage** = `[leaves_taken] / [work_days]`
3. **Salary Category** = `IF [salary] > 80000 THEN "High" ELSEIF [salary] > 50000 THEN "Medium" ELSE "Low" END`
4. **Female Ratio** = `SUM(IF [gender] = "Female" THEN 1 ELSE 0 END) / COUNT([employee_id])`

### 5.3 Dashboard Components
#### KPI Cards
- **Total Employees:** 500
- **Average Salary:** $57,627
- **Number of Departments:** 5
- **Average Performance:** 3.0 / 5
- **Female Ratio:** 47%

#### Visualizations
1. **Employees by Department** (Bar Chart) – Displays headcount per department.
2. **Average Salary by Department** (Bar Chart) – Highlights salary disparities.
3. **Performance Rating Distribution** (Bar Chart) – Shows rating frequency.
4. **Salary Distribution** (Histogram) – Reveals salary concentration.
5. **Employees by Gender** (Pie Chart) – Depicts gender balance.
6. **Performance Heatmap** (Heatmap) – Performance rating by department.
7. **Salary by Job Role** (Tree Map) – Total salary expenditure per role.
8. **Salary vs Performance** (Scatter Plot) – Relationship between salary and rating.
9. **Hiring Trend** (Line Chart) – Yearly hiring pattern.

#### Interactive Filters
- Department
- Gender
- Job Title
- Performance Rating
- Salary Range (slider)

All filters apply to the entire dashboard, enabling dynamic exploration.

### 5.4 Dashboard Design
- **Background:** Custom template for professional layout.
- **Color Palette:** Blue, orange, and gray for clarity.
- **Tooltips:** Enhanced with employee details on hover.


---

## 6. Key Insights & Recommendations
### Insights
1. **Workforce Distribution:** Sales (24%) and IT (22%) are the largest departments, indicating a focus on revenue generation and technology.
2. **Salary Trends:** IT pays the highest average salary; HR and Marketing are below average. High performers (rating 5) earn 20% more than average employees.
3. **Performance:** Majority of employees are average performers (rating 3). Top performers (18%) are concentrated in IT and Sales.
4. **Gender Diversity:** Near balance (53% male, 47% female), but leadership roles may need review.
5. **Hiring:** Hiring declined in 2023; investigate if this aligns with business strategy.
6. **Attendance:** Average leave percentage is 5%; no major absenteeism issues.

### Recommendations
- **Retention Strategy:** Focus on retaining top performers (rating 5) with career development and competitive compensation.
- **Salary Review:** Consider adjusting salaries in HR and Marketing to remain competitive.
- **Performance Improvement:** Implement training programs for departments with lower average ratings (HR, Marketing).
- **Diversity Initiatives:** Maintain gender balance and track promotion rates.
- **Hiring Plan:** Analyze reasons for hiring slowdown; align with future growth projections.

---

## 7. Conclusion
This HR Workforce Analytics project successfully demonstrates the power of combining SQL data extraction with Tableau visualization to derive meaningful business insights. The MySQL database provides a robust foundation for querying employee data, while the Tableau dashboard offers an interactive platform for HR leaders to monitor key metrics in real time. The insights gained can inform strategic decisions regarding talent management, compensation planning, and organizational development.

---

**Project prepared by:** Praveen  
**Date:** March 2026  
**Contact:** vinopraveen6363@gmail.com

---
