# Student-Academic-Performance-Dashboard-Power-BI--Parectical
Interactive Power BI dashboard for analyzing student academic performance, attendance, behavior, subject-wise scores, and term-wise performance using DAX, data modeling, slicers, and drill-through analysis.
# 📊 Student Academic Performance Dashboard — Power BI

An end-to-end Power BI project that analyzes student academic performance using data from **students, scores, attendance, and behavior** records. The dashboard delivers interactive insights into academic performance, attendance patterns, behavioral trends, and individual student profiles — enabling teachers, coordinators, and school administrators to make data-driven decisions.

---

## 📁 Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Dataset Description](#dataset-description)
- [Data Model](#data-model)
- [Key Features](#key-features)
- [Dashboard Pages](#dashboard-pages)
- [DAX Measures](#dax-measures)
- [Tools & Technologies](#tools--technologies)
- [Project Workflow](#project-workflow)
- [Key Insights](#key-insights)
- [How to Use This Project](#how-to-use-this-project)
- [Folder Structure](#folder-structure)
- [Future Enhancements](#future-enhancements)
- [Author](#author)

---

## 📌 Project Overview

This Power BI project analyzes student academic performance by combining four core datasets — **Students**, **Scores**, **Attendance**, and **Behavior**. It transforms raw CSV data into a fully interactive, multi-page report that highlights academic strengths, weaknesses, attendance patterns, and behavioral trends at both the class level and the individual student level.

The report is designed to answer real questions school stakeholders actually ask:
- Which students are excelling, and which need intervention?
- How does performance vary across subjects, classes, and terms?
- Is there a correlation between attendance, behavior, and academic scores?
- What does a single student's complete academic journey look like?

---

## 🎯 Business Problem

Educational institutions generate large volumes of data across academics, attendance, and behavior — but this data is often siloed in separate spreadsheets, making it hard to:

- Spot at-risk students early
- Compare performance fairly across classes/sections
- Track improvement or decline over multiple terms
- Correlate non-academic factors (attendance, behavior) with academic outcomes

This dashboard solves that by unifying all four datasets into a single interactive model with drill-through capability down to the individual student.

---

## 🗂️ Dataset Description

| Dataset | Description | Key Fields |
|---|---|---|
| **Students** | Master list of student demographic and enrollment data | Student ID, Name, Class, Section, Gender, Admission Date |
| **Scores** | Subject-wise, term-wise exam scores | Student ID, Subject, Term, Marks Obtained, Max Marks |
| **Attendance** | Daily/monthly attendance records | Student ID, Date, Status (Present/Absent/Late) |
| **Behavior** | Logged behavioral incidents/remarks | Student ID, Date, Behavior Type, Remarks, Severity |

> All datasets are provided as CSV files and connected via **Student ID** as the common key.

---

## 🧩 Data Model

The data model follows a **star schema** approach for optimal performance and clean DAX:

```
                Students (Dimension)
                       |
        --------------------------------
        |              |               |
    Scores (Fact)  Attendance (Fact)  Behavior (Fact)
```

- **Students** acts as the central dimension table.
- **Scores**, **Attendance**, and **Behavior** are fact tables, each related to Students via a one-to-many relationship on `Student ID`.
- A separate **Date/Calendar table** is used to enable proper time intelligence across terms and months.
- A **Subject** and **Term** dimension table (if normalized) helps slice performance cleanly without duplication.

---

## ⚙️ Key Features

- ✅ Student performance analysis (individual & aggregate)
- ✅ Subject-wise score comparison across classes
- ✅ Term-wise performance trend tracking
- ✅ Attendance analysis (present %, absentee trends, chronic absenteeism flags)
- ✅ Behavior distribution and incident tracking
- ✅ KPI cards for at-a-glance metrics (Avg Score, Attendance %, Pass %, Total Students)
- ✅ Interactive slicers (Class, Section, Subject, Term, Gender)
- ✅ Student drill-through profile page
- ✅ Conditional formatting (color-coded performance bands: high/average/low)
- ✅ Custom DAX measures for dynamic calculations

---

## 📑 Dashboard Pages

### 1️⃣ Academic Performance Overview
- KPI cards: Average Score, Pass %, Top Performer, Total Students
- Subject-wise average score comparison (bar/column chart)
- Term-over-term performance trend (line chart)
- Class/Section-wise performance comparison
- Score distribution histogram
- Slicers: Class, Section, Term, Subject

### 2️⃣ Student Profile / Drill-Through
- Individual student header card (Name, Class, Section, Photo placeholder)
- Subject-wise score breakdown for the selected student
- Term-wise performance trend line for that student
- Attendance summary (Present %, Absent days)
- Behavior remarks/history log
- Accessed via right-click drill-through from any visual on the Overview page

### 3️⃣ Behavioral Analysis
- Behavior type distribution (pie/donut chart)
- Behavior incidents over time (trend chart)
- Behavior vs. Academic Performance correlation visual
- Class/Section-wise behavior comparison
- Top students with most/least behavioral incidents

---

## 🧮 DAX Measures

Examples of core DAX measures used in the report:

```DAX
Average Score = AVERAGE(Scores[Marks Obtained])

Pass Percentage = 
DIVIDE(
    CALCULATE(COUNTROWS(Scores), Scores[Marks Obtained] >= Scores[Passing Marks]),
    COUNTROWS(Scores)
)

Attendance Percentage = 
DIVIDE(
    CALCULATE(COUNTROWS(Attendance), Attendance[Status] = "Present"),
    COUNTROWS(Attendance)
)

Top Performer = 
CALCULATE(
    MAX(Students[Name]),
    TOPN(1, Scores, Scores[Marks Obtained], DESC)
)

Performance Band = 
SWITCH(
    TRUE(),
    [Average Score] >= 85, "High Performer",
    [Average Score] >= 60, "Average Performer",
    "Needs Improvement"
)

Behavior Incident Count = COUNTROWS(Behavior)

Term-over-Term Change = 
[Average Score] - CALCULATE([Average Score], PREVIOUSMONTH('Date'[Date]))
```

> These measures power the KPI cards, conditional formatting rules, and trend visuals throughout the report.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Report building & visualization |
| **Power Query (M)** | Data cleaning, transformation, and loading |
| **DAX** | Custom measures, calculated columns, KPIs |
| **Data Modeling** | Star schema relationships between fact & dimension tables |
| **CSV Files** | Raw data source (Students, Scores, Attendance, Behavior) |

---

## 🔄 Project Workflow

1. **Data Collection** — Gathered raw CSV files for Students, Scores, Attendance, and Behavior.
2. **Data Cleaning (Power Query)** — Handled missing values, standardized formats, removed duplicates, and fixed data types.
3. **Data Modeling** — Built relationships between fact and dimension tables in a star schema; created a Date table for time intelligence.
4. **DAX Development** — Wrote measures for KPIs, percentages, performance bands, and trend calculations.
5. **Report Design** — Built three interactive pages with slicers, visuals, conditional formatting, and drill-through navigation.
6. **Testing & Validation** — Verified calculations, cross-filtering behavior, and drill-through accuracy.
7. **Publishing** — Report ready for publishing to Power BI Service (optional) for sharing and scheduled refresh.

---

## 💡 Key Insights

- Identify high and low-performing students at a glance using color-coded performance bands.
- Compare performance across subjects and classes to spot curriculum or teaching gaps.
- Track academic trends across terms to measure improvement or decline over time.
- Analyze attendance and behavioral patterns to flag students who may need additional support.
- Filter every insight dynamically by class, section, subject, and term for targeted analysis.

---

## ▶️ How to Use This Project

1. Clone or download this repository.
2. Open the `.pbix` file in **Power BI Desktop** (latest version recommended).
3. If prompted, update the data source file paths to point to your local CSV files.
4. Click **Refresh** in the Home ribbon to load the latest data.
5. Use the slicers on each page to filter by Class, Section, Subject, or Term.
6. Right-click any student's row/bar on the Overview page → **Drill Through** → **Student Profile** to view an individual report.

---

## 📂 Folder Structure

```
student-performance-dashboard/
│
├── data/
│   ├── students.csv
│   ├── scores.csv
│   ├── attendance.csv
│   └── behavior.csv
│
├── report/
│   └── Student_Performance_Dashboard.pbix
│
├── screenshots/
│   ├── overview_page.png
│   ├── student_profile_page.png
│   └── behavioral_analysis_page.png
│
└── README.md
```

---

## 🚀 Future Enhancements

- Add predictive analytics (e.g., at-risk student prediction using trend-based scoring)
- Integrate a mobile-optimized report layout
- Add Row-Level Security (RLS) for teacher-specific or class-specific data access
- Automate data refresh via Power BI Service with a scheduled gateway connection
- Add parent/guardian view with restricted, single-student access

---

## 👤 Author

**Montu**
Solo Digital Entrepreneur & Data Analytics Enthusiast

---

*If you find this project useful, consider ⭐ starring the repository.*
