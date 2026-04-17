---

---

<p align="center">

<img src="logo.png">

</p>
# Advanced Power BI Examination: Healthcare Analytics

**Course:** DSA 3050A SS 2026 End Semester Exam
**Project Title:** Strategic Healthcare Analytics: Predictive Modeling of Patient No-Shows
**Student Name:** Calvin Gacheru Mwangi
**Admission Number:** 670035
**GitHub Repository:** [Insert your Link here]

---
## Introduction
Patient no-shows represent a significant operational challenge in healthcare administration, leading to idle clinical resources and delayed care. This report outlines the development of a complete Power BI analytical solution designed to address this inefficiency.

Using a raw dataset of over 110,000 medical records, this project executes rigorous data cleaning and transformation to build a robust relational model. The final interactive dashboard utilizes advanced DAX calculations to uncover demographic, geographic, and scheduling patterns correlated with missed appointments. The objective is to deliver data-driven insights and actionable recommendations that optimize operational efficiency and improve patient triage.

---
## **Part A: Data Acquisition and Understanding**
**1. Domain and Business Problem**
- Domain: Healthcare Analytics.
- Business Problem: Patient no-shows cause idle clinical resources and operational bottlenecks. The objective is to analyze historical appointment data to identify demographic, geographic, and scheduling patterns that correlate with missed appointments, enabling targeted interventions and optimized scheduling.

**2. Dataset Source**
Source: Kaggle. The dataset is titled ["Medical Appointment No-Shows"](https://www.kaggle.com/datasets/joniarroba/noshowappointments)
(originally sourced from public medical records in Brazil).

![alt text](<kaggle data.png>)

**3. Key Tables and Fields**
The raw dataset is a single flat file containing 110,527 rows.
Key variables include:
* Identifiers: `PatientId`, `AppointmentID`
* Demographics: `Gender`, `Age`, `Neighbourhood`
* Timestamps: `ScheduledDay`, `AppointmentDay`
* Health Conditions: `Hipertension`, `Diabetes`, `Alcoholism`, `Handcap`
* Operational: `Scholarship` (welfare enrollment), `SMS_received`, `No-show` (target variable)

To fulfill the relational model requirement, this data will be transformed in Power Query into a star schema containing:
* DimPatient: `PatientId`, demographics, and health conditions.
* DimLocation: `Neighbourhood`.
* DimDate: Derived from the timestamp fields.
* FactAppointments: `AppointmentID`, `PatientId`, location/date keys, `SMS_received`, and `No-show` status.

**4. Suitability for Advanced Power BI Analysis**
This dataset is suitable for advanced Power BI analysis due to:
* Volume: Contains over 110,000 rows, exceeding the 9,000 minimum.
* Complexity: Includes categorical, numerical, boolean, and datetime variables.
* Transformation Needs: The raw data is messy. It requires text standardization, splitting datetime strings, handling invalid records (e.g., negative ages), and unpivoting/splitting columns to build the required relational tables.

View of the raw data in Power Query Editor:
![View of the data](data.png)

---
## Part B: Data cleaning and transformations
### Data Cleaning
- Before cleaning
![[Pasted image 20260415144325.png]]
- Removed 2 in Handicapped columns since it is yes(1) or no(0)
![[Pasted image 20260415144837.png]]

- Changed PatientID and AppointmentId datatype to `text` from whole number - you do not perform mathematical operations on IDs
![[Pasted image 20260415145213.png]]
- After cleaning
![[Pasted image 20260415145459.png]]

### Data Transformation
- Extract date part - This creates a new column showing Monday, Tuesday, etc., which is crucial for analyzing no-show trends by weekday.
![[Pasted image 20260417083829.png]]
- Age-bins - grouping ages
![[Pasted image 20260417084221.png]]
- Replace values - `No-show` with 1 for yes and 0 for no
![[Pasted image 20260417084730.png]]
- Time bins
![[Pasted image 20260417113927.png]]
- Changed data type
![[Pasted image 20260417085117.png]]
- After transformations
![[Pasted image 20260417085912.png]]

### Building a star schema
- Create DimPatient
![[Pasted image 20260417090502.png]]
- DimLocation
![[Pasted image 20260417091052.png]]
- Merge queries
![[Pasted image 20260417091458.png]]
![[Pasted image 20260417092305.png]]
- Relationship between tables
![[Pasted image 20260417095636.png]]

---
## Part C: Data Modeling

- Creating a Date table
```DAX
DimDate = CALENDAR(MIN(FactAppointments[AppointmentDate]), MAX(FactAppointments[AppointmentDate]))
```
![[Pasted image 20260417093316.png]]

- Marking as data
![[Pasted image 20260417093907.png]]


### **Identify and Justify Tables**

- **Fact Table:** `FactAppointments`. Justification: It contains the quantitative events (the appointments) and the measurable target variable (`No_Show`).
    
- **Dimension Tables:** `DimPatient`, `DimLocation`, `DimDate`. Justification: They contain the descriptive text attributes (demographics, neighborhood names, time periods) used to filter, slice, and group the metrics in the fact table.

### **Relationships, Cardinality, and Cross-filter Direction*

- `DimPatient[PatientId]` (1) to `FactAppointments[PatientId]` (*)
    
- `DimLocation[LocationID]` (1) to `FactAppointments[LocationID]` (*)
![[Pasted image 20260417095907.png]]
- `DimDate[Date]` (1) to `FactAppointments[AppointmentDate]` (*). This is your primary, active relationship.
![[Pasted image 20260417095815.png]] 
- `DimDate[Date]` (1) to `FactAppointments[ScheduledDate]` (*). 
![[Pasted image 20260417095748.png]]

### **Model Structure Efficiency**

This structure supports efficient reporting because it forms a clean star schema. The single-directional 1:* relationships ensure filters flow downhill from dimensions to facts without creating circular dependencies. It reduces data redundancy, compresses the file size, and allows DAX engine queries to run significantly faster than they would on a flat table.


**Explanation of Each Table's Role**

- **FactAppointments (Fact Table):** Stores the transactional events (individual appointments) and the measurable target variable (`No_Show`). It contains foreign keys used to link to the surrounding dimension tables.
    
- **DimPatient (Dimension Table):** Stores descriptive attributes about individual patients, including demographics (Gender, Age Group) and health conditions (Hypertension, Diabetes). It is used to slice and filter fact table metrics by patient profiles.
    
- **DimLocation (Dimension Table):** Stores geographic data (`Neighbourhood`). It is used to analyze spatial distributions and identify locations with high no-show rates.
    
- **DimDate (Dimension Table):** A continuous calendar table required for Power BI time intelligence functions. It enables the analysis of trends over days, weeks, and months.
    

**Relationship Description** The model utilizes a star schema optimized for Power BI. All relationships flow downstream from dimensions to the central fact table.

- **DimPatient to FactAppointments:** A 1-to-many (1:*) relationship linked via `PatientId` with a single cross-filter direction.
    
- **DimLocation to FactAppointments:** A 1-to-many (1:*) relationship linked via `LocationID` with a single cross-filter direction.
    
- **DimDate to FactAppointments (Active):** A 1-to-many (1:*) relationship linked via the `Date` and `AppointmentDate` columns with a single cross-filter direction.
    
- **DimDate to FactAppointments (Inactive):** A 1-to-many (1:*) relationship linked via the `Date` and `ScheduledDate` columns. This is kept inactive to prevent circular dependencies while allowing activation via DAX (`USERELATIONSHIP`) if analysis based on scheduling date is required.

![[Pasted image 20260417101020.png]]

---
## Part D: DAX Measures and Calculated columns
### Calculated columns
1) WaitTimeDays
```Dax
WaitTimeDays = DATEDIFF(FactAppointments[ScheduledDate], FactAppointments[AppointmentDate], DAY)
```
![[Pasted image 20260417101211.png]]
- **What it calculates:** The number of days between when the patient booked the appointment and the actual appointment date.
    
- **Decision-making value:** Helps identify if patients booking far in advance are more likely to miss their appointments compared to same-day bookings.
2) Risk Level
```DAX
Risk Level = 
SWITCH(
    TRUE(),
    FactAppointments[WaitTimeDays] == 0, "Low Risk (Same Day)",
    FactAppointments[WaitTimeDays] <= 7, "Medium Risk (1-7 Days)",
    "High Risk (8+ Days)"
)
```
![[Pasted image 20260417101430.png]]

* ***What it calculates:** Categorizes the appointment into risk buckets based on the wait time using conditional `SWITCH` logic.
    
- **Decision-making value:** Allows administrators to segment patients and target high-risk (8+ days) appointments with automated SMS reminders.

### DAX Measures
**1. Total Appointments (Count)**
```
Total Appointments = COUNTROWS(FactAppointments)
```
![[Pasted image 20260417101747.png]]
- **What it calculates:** The total number of appointment records in the dataset.
- **Decision-making value:** Provides the baseline volume of clinic activity required to contextualize all other metrics.
    
**2. Total No-Shows (Total Amount)**
```
Total No-Shows = SUM(FactAppointments[No_Show])
```
![[Pasted image 20260417101832.png]]
- **What it calculates:** Adds up the `1`s in the `No_Show` column.
- **Decision-making value:** Quantifies the exact number of missed appointments, showing the raw scale of the problem.

**3. Distinct Patients (Distinct Count)**
```
Distinct Patients = DISTINCTCOUNT(FactAppointments[PatientId])
```
![[Pasted image 20260417101947.png]]
- **What it calculates:** The number of unique individuals who visited the clinic, ignoring multiple visits by the same person.
- **Decision-making value:** Shows whether no-shows are driven by a large group of people missing one appointment, or a small group of repeat offenders missing multiple.

**4. No-Show Rate % (Percentage Contribution)**
```
No-Show Rate = DIVIDE([Total No-Shows], [Total Appointments], 0)
```
![[Pasted image 20260417102103.png]]
- **What it calculates:** The ratio of missed appointments to total booked appointments.
- **Decision-making value:** This is the primary Key Performance Indicator (KPI). It normalizes performance so you can compare different neighborhoods or months fairly regardless of total volume.

**5. Average Wait Time (Average Metric)**
```
Average Wait Time = AVERAGE(FactAppointments[WaitTimeDays])
```
![[Pasted image 20260417102209.png]]
- **What it calculates:** The mean number of days patients wait for their appointments.
- **Decision-making value:** Highlights operational bottlenecks. If this number climbs, the clinic is struggling to meet patient demand promptly.

**6. YTD Total Appointments (Time Intelligence)**
```
YTD Total Appointments = TOTALYTD([Total Appointments], DimDate[Date])
```
![[Pasted image 20260417102321.png]]
- **What it calculates:** The cumulative sum of appointments from January 1st to the current context date.
- **Decision-making value:** Allows management to track cumulative annual volume against yearly operational targets.

**7. MoM No-Show Growth % (Change Over Time)**
```
MoM No-Show Growth = 
VAR PrevMonth = CALCULATE([Total No-Shows], DATEADD(DimDate[Date], -1, MONTH))
RETURN DIVIDE([Total No-Shows] - PrevMonth, PrevMonth, 0)
```
![[Pasted image 20260417102417.png]]
- **What it calculates:** The percentage increase or decrease in raw no-shows compared to the previous month.
- **Decision-making value:** Acts as an early warning system. A positive growth percentage indicates the no-show problem is worsening and requires immediate intervention.

**8. Location No-Show Rank (Ranking)**
```
Location Rank = RANKX(ALL(DimLocation[Neighbourhood]), [Total No-Shows], , DESC, Dense)
```
![[Pasted image 20260417102618.png]]
- **What it calculates:** Assigns a rank (1, 2, 3...) to each neighborhood based on its total number of no-shows.
- **Decision-making value:** Directs geographic resource allocation. It tells clinic staff exactly which neighborhood to target first for community outreach or targeted reminders.

---
## Part E:  Dashboard Design and Visualization (20 Marks)

### Page1: Executive summary
![[Pasted image 20260417125654.png]]
### Page2: Detailed Analysis
![[Pasted image 20260417125619.png]]
### Page3: Insights and performance monitor
![[Pasted image 20260417125533.png]]

- **Explanation of Design Choices**: The dashboard utilizes a top-down information architecture. High-level KPIs are positioned at the top left, matching natural reading patterns. A minimal, high-contrast color theme is applied to avoid visual clutter, using a designated warning color (red) exclusively for negative metrics like no-show volume. Visuals are aligned on a strict grid layout to maintain professional aesthetics. Slicers are positioned uniformly across pages to establish a predictable user interface.

- **Explanation of User Interactions**: The dashboard relies on bidirectional cross-filtering. Selecting a specific demographic category (e.g., clicking the 'Senior' bar) dynamically updates all other visuals on the page to reflect that cohort's metrics. Slicers for Date and Location are synchronized across all three pages to maintain context during navigation. The Detailed Analysis page utilizes hierarchical drill-down functionality, allowing users to unpack annual data into monthly and daily trends, while the Decomposition Tree enables ad-hoc root cause analysis by letting users expand specific branches of the data model. Tooltips are enabled on all visual elements to provide exact numeric values upon hover.
---
## **Part F: Analytical Insights and Business Recommendations**

**Analytical Insights**
1. **Lead Time Correlation:** The decomposition tree and matrix visuals prove a severe correlation between booking lead time and absenteeism. High-risk appointments (booked 8+ days in advance) suffer a 32% no-show rate, while same-day bookings hold a minimal 5% no-show rate.
2. **The SMS Paradox:** The decomposition tree reveals a critical systemic failure: patients who received an SMS reminder exhibit a higher no-show rate (28%) than those who did not (17%). The current SMS strategy is either targeting the wrong appointments or is timed ineffectively.
3. **Demographic Concentration:** Adult females drive the majority of absenteeism. Females account for 65.38% of all missed appointments, and the "Adult" category dwarfs Youth, Children, and Seniors in total no-show volume.
4. **Time-of-Day Attrition:** Evening appointments experience the highest absolute volume of no-shows compared to mornings and afternoons, pointing to end-of-day scheduling conflicts or fatigue.
5. **Clinical Profile:** Absenteeism is not driven by chronic health complications. Over 83% of no-shows have no hypertension, 93% have no diabetes, and 98% have no handicap. Healthy individuals are missing appointments due to logistical barriers, not medical ones.

**Actionable Recommendations**
1. **Implement Predictive Overbooking:** Apply a strict overbooking multiplier to evening slots and appointments booked 8+ days in advance to counteract the predictable 32% revenue/time loss in these specific segments.
2. **Overhaul the SMS Architecture:** Immediately suspend and rebuild the SMS reminder protocol. Shift from automated blanket messaging to targeted reminders sent exactly 24 hours prior, focusing specifically on the high-risk adult female demographic.   
3. **Shift Capacity to Same-Day Triage:** Since same-day appointments guarantee a 95% attendance rate, restructure clinic schedules to reserve a larger block of daily capacity for walk-ins and short-notice bookings, reducing the reliance on high-attrition, long-lead scheduling.

---
## Conclusion
This project successfully delivered a comprehensive Business Intelligence solution to analyze and mitigate medical appointment no-shows. By transforming over 110,000 raw medical records into a functional star-schema relational model, we created a robust foundation for advanced analytics. The application of DAX-driven KPIs, such as the No-Show Rate and Risk Level logic, allows for real-time monitoring of clinical efficiency.

The resulting multi-page dashboard provides stakeholders with the tools necessary to identify high-risk scheduling windows and demographic attrition patterns. Implementing the proposed data-driven recommendations—specifically targeted overbooking and the restructuring of the SMS notification architecture—will enable the clinic to optimize resource allocation and improve patient health outcomes.