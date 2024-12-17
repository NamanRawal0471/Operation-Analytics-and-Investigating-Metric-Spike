### **Operation Analytics and Investigating Metric Spike** 📊  

This project focuses on utilizing **Advanced SQL** techniques to analyze operational data, identify areas for improvement, and investigate sudden spikes in key metrics. By working with datasets related to job reviews, user activities, and email events, this project provides actionable insights for **process optimization** and business decision-making.

---

## **Overview**  
Operational Analytics is critical for understanding end-to-end business operations, ensuring efficiency, and addressing unexpected changes in key performance indicators. As a **Lead Data Analyst**, the goal is to investigate metrics, derive insights, and deliver solutions to help teams like **operations, marketing**, and **support** make data-driven decisions.

The project is divided into **two major case studies**:  
1. **Job Data Analysis** – Focuses on analyzing job-related metrics, identifying duplicates, and understanding throughput.  
2. **Investigating Metric Spikes** – Explores user growth, retention, and engagement to identify changes and anomalies.

---

## **Case Study 1: Job Data Analysis**  

### **Tasks & SQL Queries**  

1. **Jobs Reviewed Over Time**  
   Objective: Calculate the number of jobs reviewed **per hour** for each day in November 2020.  
   ```sql
   SELECT ds, HOUR(time_spent) AS hour, COUNT(*) AS jobs_reviewed
   FROM job_data
   WHERE ds BETWEEN '2020-11-01' AND '2020-11-30'
   GROUP BY ds, hour
   ORDER BY ds, hour;
   ```

2. **Throughput Analysis**  
   Objective: Calculate the **7-day rolling average** of throughput (number of events per second).  
   ```sql
   SELECT ds, 
          AVG(event_count / time_spent) OVER (ORDER BY ds ROWS 6 PRECEDING) AS rolling_avg_throughput
   FROM (SELECT ds, COUNT(event) AS event_count, SUM(time_spent) AS time_spent 
         FROM job_data 
         GROUP BY ds) AS daily_metrics;
   ```

3. **Language Share Analysis**  
   Objective: Calculate the **percentage share** of each language over the last 30 days.  
   ```sql
   SELECT language, 
          COUNT(*) * 100.0 / SUM(COUNT(*)) OVER() AS percentage_share
   FROM job_data
   WHERE ds >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
   GROUP BY language;
   ```

4. **Duplicate Rows Detection**  
   Objective: Identify duplicate rows in the data.  
   ```sql
   SELECT job_id, actor_id, event, language, COUNT(*) AS duplicate_count
   FROM job_data
   GROUP BY job_id, actor_id, event, language
   HAVING COUNT(*) > 1;
   ```

---

## **Case Study 2: Investigating Metric Spike**  

### **Tasks & SQL Queries**  

1. **Weekly User Engagement**  
   Objective: Measure the activeness of users on a weekly basis.  
   ```sql
   SELECT YEARWEEK(event_date) AS week, COUNT(DISTINCT user_id) AS active_users
   FROM events
   GROUP BY YEARWEEK(event_date);
   ```

2. **User Growth Analysis**  
   Objective: Analyze the **growth of users** over time.  
   ```sql
   SELECT YEARWEEK(signup_date) AS signup_week, COUNT(*) AS new_users
   FROM users
   GROUP BY signup_week
   ORDER BY signup_week;
   ```

3. **Weekly Retention Analysis**  
   Objective: Analyze weekly user retention after sign-up.  
   ```sql
   SELECT signup_week, current_week, 
          COUNT(DISTINCT user_id) AS retained_users
   FROM (
       SELECT user_id, YEARWEEK(signup_date) AS signup_week, 
              YEARWEEK(event_date) AS current_week
       FROM users u
       JOIN events e ON u.user_id = e.user_id
   ) AS retention_data
   GROUP BY signup_week, current_week
   ORDER BY signup_week, current_week;
   ```

4. **Weekly Engagement Per Device**  
   Objective: Measure user engagement **per device** on a weekly basis.  
   ```sql
   SELECT device_type, YEARWEEK(event_date) AS week, COUNT(DISTINCT user_id) AS weekly_engagement
   FROM events
   GROUP BY device_type, week;
   ```

5. **Email Engagement Analysis**  
   Objective: Analyze how users are engaging with emails.  
   ```sql
   SELECT email_status, COUNT(*) AS email_count
   FROM email_events
   GROUP BY email_status;
   ```

---

## **Tools & Technologies Used**  
- **MySQL Workbench 8.0 CE**: Used for executing queries and managing the database.  
- **SQL**: Core language for querying and analyzing data.  
- **CSV Data**: Imported into MySQL for analysis.  

---

## **Insights Gained**  

1. **Job Analysis**  
   - High job throughput patterns and duplicate detection ensure better operational monitoring.  
   - Identifying rolling averages smooths fluctuations for better decision-making.  

2. **Metric Spike Investigation**  
   - Weekly retention analysis provides insights into user behavior post sign-up.  
   - Email engagement analysis identifies how users interact with email services, guiding improvements.  

3. **Data-Driven Decisions**  
   - Accurate calculations of engagement and growth enable better product and operational strategies.  

---

## **Results**  
The project successfully provided insights into operational efficiency, user behavior, and anomaly detection. Advanced SQL techniques were applied to resolve real-world business queries, ensuring actionable solutions.

---

## **How to Run the Project**  
1. Import the provided CSV files into MySQL Workbench.  
2. Execute the SQL queries mentioned above to analyze the datasets.  
3. Document the outputs and insights in a structured report (PDF/PPT).  

---

## **Tech Stack**  
- **SQL**  
- **MySQL Workbench**  

---

### **Key Learnings**  
This project enhanced my skills in advanced SQL techniques, problem-solving for metric spikes, and delivering actionable business insights.  

---  
**By**: Naman Rawal 🚀  
**Role**: Data Analytics Trainee
