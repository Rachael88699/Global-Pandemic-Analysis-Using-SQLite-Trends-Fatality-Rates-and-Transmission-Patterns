#  Global Pandemic Analysis Using SQLite

##  Introduction
Pandemics have significantly shaped human history through widespread health, economic, and social impacts.  
This project analyzes a dataset of historical pandemics using **SQLite** to uncover patterns in transmission, fatality rates, geographic spread, and duration.

The goal is to transform raw data into meaningful insights using structured queries.

---

##  About the Dataset
The dataset contains records of major pandemics across different time periods, including:

- Event Name  
- Pathogen Type  
- Start and End Years  
- Duration  
- Origin Region  
- Geographic Spread  
- Estimated Cases and Deaths  
- Case Fatality Rate  
- Transmission Methods  
- Containment Measures  
- Economic Impact  
- Spread Score
- era

 **Dataset Source:**  
https://1drv.ms/x/c/f729756d9dbf5534/IQDytaZSbLKLTbvg7lv35gGOAUDDTbw6Lr7UV4koQ7yAY8Y?e=s1XS3B

---

## Problem Statement
Despite the availability of historical pandemic data, it is often not fully analyzed to extract actionable insights.

This project answers key questions such as:
- Which pandemics were the most deadly?
- What pathogen types are most common?
- How do transmission methods affect spread?
- Which regions are most affected?
- What factors influence fatality rates?

---

##  Tools Used
- **SQLite** – Data storage and querying  
- **DB Browser for SQLite** – Query execution and data exploration  
- **Excel / CSV** – Data source  
- **GitHub** – Documentation and version control  

---

##  Visualization (Dashboard)
The project uses a **tabular dashboard (SQLite table screenshot)** to display:

- Structured pandemic records  
- Cleaned dataset columns  
- Organized data for querying  

![](Dashboard_Image1.jpeg)
![](Dashboard_Image2.jpeg)
![](Dashboard_Image3.jpeg)
![](Dashboard_Image4.jpeg)
![](Dashboard_Image5.jpeg)
![](Dashboard_Image6.jpeg)
![](Dashboard_Image7.jpeg)
![](Dashboard_Image8.jpeg)
![](Dashboard_Image9.jpeg)
![](Dashboard_Image10.jpeg)

---

##  SQL Analysis Queries

### 1. Most Deadly Pandemics
```sql
SELECT event_name, 
       CAST(Case_Fatality_Rate_Pct AS REAL) AS fatality_rate
FROM raw_data
ORDER BY fatality_rate DESC;
LIMIT 10;

### 2. Number Of Pandemics Per Century
```sql
SELECT century, COUNT(*) AS number_of_events
FROM raw_data
GROUP BY century
ORDER BY number_of_events DESC;

### 3. Distribution Of Pathogen Types
```sql
SELECT pathogen_type, COUNT(*) AS number_of_events
FROM raw_data
GROUP BY pathogen_type
ORDER BY number_of_events DESC;

###  3. Transmission Method Analysis
```sql
SELECT primary_transmis, COUNT(*) AS number_of_events
FROM raw_data
GROUP BY primary_transmis
ORDER BY number_of_events DESC;

### 4. Containment Method Analysis
```sql
SELECT containment_meth, COUNT(*) AS number_of_events
FROM raw_data
GROUP BY containment_meth
ORDER BY number_of_events DESC;

### 5. Longest Pandemics
```sql
SELECT event_name, duration_years
FROM raw_data
ORDER BY duration_years DESC;

### 6.Average Fatality Rate by Pathogen Type 
```sql
SELECT pathogen_type,
       COUNT(*) AS number_of_events,
       AVG(CAST(Case_Fatality_Rate_Pct AS REAL)) AS avg_fatality_rate
FROM raw_data
GROUP BY pathogen_type
ORDER BY avg_fatality_rate DESC;

## 7. Spread Vs Death Comparison
```sql
SELECT event_name, spread_score, estimated_deaths
FROM raw_data
ORDER BY spread_score DESC;

## 8. Origin Region Distribution
```sql
SELECT origin_region, COUNT(*) AS number_of_events
FROM raw_data
GROUP BY origin_region
ORDER BY number_of_events DESC;

## 9. Economic Impact Ranking
```sql
SELECT event_name, CAST(economic_impact_b AS REAL) AS economic_impact
FROM raw_data
ORDER BY economic_impact DESC;

## 10. Events Per Year (Time Analysis)
```sql
WITH RECURSIVE years(year) AS (
    SELECT MIN(start_year) FROM raw_data
    UNION ALL
    SELECT year + 1 FROM years
    WHERE year < (SELECT MAX(end_year) FROM raw_data)
)
SELECT y.year,
       COUNT(r.event_name) AS number_of_events
FROM years y
LEFT JOIN raw_data r
ON y.year BETWEEN r.start_year AND COALESCE(r.end_year, r.start_year)
GROUP BY y.year
ORDER BY y.year;

## Key Insights
Pandemics with high fatality rates are often associated with specific pathogen types
Viral diseases are the most frequent cause of pandemics
Airborne transmission leads to rapid and wide spread
Some regions repeatedly appear as origins of outbreaks
Spread rate does not always correlate with death rate
Longer pandemics are not necessarily the deadliest

## Recommendations
--Strengthen early detection and surveillance systems
--Prioritize control of airborne diseases
--Improve global health collaboration
--Invest in healthcare infrastructure
--Maintain clean and standardized datasets for analysis

## Conclusion
This project demonstrates how SQL can be used to analyze real-world datasets and generate meaningful insights.
By exploring pandemic trends, we gain valuable knowledge that can support better decision-making in global health.

## Future Improvements
--Add visual dashboards (Power BI / Tableau)
--Include real-time pandemic data
--Build an interactive web-based dashboard
--Expand analysis with machine learning models
