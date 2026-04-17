# Strategic Healthcare Analytics: Predictive Modeling of Patient No-Shows

### Student Information
* **Student Name:** Calvin Gacheru Mwangi
* **Admission Number:** 670035
* **Course Code:** DSA 3050A

---

### Project Overview
This project involves the development of a complete Business Intelligence solution using Power BI to analyze patient attendance patterns. The solution processes over 110,000 clinical records, transforming raw data into a relational star-schema model to identify the key drivers behind medical appointment no-shows.

### Problem Statement
Patient no-shows create significant operational inefficiencies in healthcare, leading to underutilized clinical staff and delayed patient care. The goal is to identify demographic and scheduling factors that predict absenteeism to enable targeted administrative interventions.

### Dataset Description
* **Source:** Kaggle (Medical Appointment No-Shows)
* **Volume:** 110,527 records.
* **Key Fields:** Patient identifiers, appointment timestamps (Scheduled vs. Appointment day), demographics (Age, Gender, Neighborhood), and health indicators (Hypertension, Diabetes, Scholarship status).

### Tools Used
* **Power BI Desktop:** Core analytical platform
* **Power Query:** Data cleaning and ETL processes.
* **DAX:** Development of calculated columns and measures.
* **GitHub:** Version control and project documentation

### Steps Followed
1. **Data Acquisition:** Imported the raw CSV and performed initial profiling.
2. **Data Cleaning:** Addressed invalid records (negative ages), fixed typos in headers, and removed duplicate IDs.
3. **ETL & Transformation:** Split timestamps into date/time parts and generated age-group bins.
4. **Data Modeling:** Decomposed the flat file into a star schema with three dimension tables and one central fact table.
5. **DAX Development:** Created 8+ measures and 2 calculated columns for in-depth analysis.
6. **Dashboard Design:** Built a 3-page interactive report for executive and detailed insights

### Dashboard Features
* **Executive Summary:** High-level KPIs and volume trends.
* **Detailed Analysis:** Hierarchical drill-downs and decomposition trees for root-cause analysis.
* **Performance Monitor:** Geographic benchmarking and anomaly detection.

### Key DAX Measures
* **No-Show Rate:** `DIVIDE([Total No-Shows], [Total Appointments], 0)` * **MoM No-Show Growth:** Calculated using `DATEADD` and `VAR` logic to track monthly changes.
* **Risk Level (Column):** A `SWITCH` function categorizing appointments by wait-time intensity

### Key Insights
1. **Wait Time Correlation:** Appointments booked more than 8 days in advance have a 32% no-show rate compared to 5% for same-day bookings.
2. **SMS Paradox:** Patients receiving SMS reminders statistically missed more appointments, suggesting ineffective timing or targeting
3. **Demographic Focus:** Adult females constitute the highest volume of total no-shows.

### Challenges Encountered
* **Flat File Normalization:** Converting a single-table CSV into a relational model required precise indexing and merging in Power Query.
* **Timestamp Cleansing:** Raw ISO8601 date formats required complex splitting to enable time intelligence analysis.

### Conclusion
This project demonstrates that scheduling lead time is the most significant predictor of patient absenteeism By leveraging the insights from this Power BI solution, healthcare administrators can implement predictive overbooking and restructured reminder protocols to significantly reduce resource waste and improve service delivery.