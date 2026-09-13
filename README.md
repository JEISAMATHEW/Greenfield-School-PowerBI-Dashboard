Greenfield High School – Power BI Data Modeling Dashboard
Data Modeling, DAX & KPI Dashboarding in Power BI

📌 Project Overview
This project simulates a real analyst task: taking a clean, related school dataset into Power BI, connecting it into a proper data model, and building calculated columns, measures, and calculated tables to power a KPI dashboard for school leadership.

The project follows an end-to-end workflow:

Related Excel Tables → Power BI Data Model → Calculated Columns → DAX Measures → Calculated Tables → KPI Dashboard

🏢 Industry
Education

💡 Business Scenario
Greenfield High School tracks students across four separate records: enrollment details, exam marks, monthly attendance, and term fee payments. Each lives in its own sheet, connected only by StudentID. The task was to bring all four into Power BI, model the relationships correctly, and turn them into a single dashboard that leadership can use to check academic performance, attendance trends, and fee collection at a glance.

📊 Dataset
Greenfield_School_Dataset.xlsx — 4 related sheets, all joined through StudentID.

Sheet	Columns	What it holds
Students	StudentID, FirstName, LastName, Gender, DateOfBirth, Class, Section, AdmissionDate	One row per student (60 students, Class 6–10)
Marks	StudentID, Subject, Term, MarksObtained, MaxMarks	One row per student, per subject, per term
Attendance	StudentID, Month, DaysPresent, TotalDays	One row per student, per month (April–September)
Fees	StudentID, Term, FeeAmount, AmountPaid, PaymentDate	One row per student, per term, with partial payments

🔗 Data Modeling
All 4 tables imported via Get Data → Excel workbook and related on StudentID in Model view
Every relationship set to One-to-Many, with Students on the "one" side
This lets measures on Marks, Attendance, and Fees be sliced by student attributes like Class, Section, and School Stage

🧮 Part A — Calculated Columns
Table	Column	Formula	What it does
Students	Age	DATEDIFF(Students[DateOfBirth], TODAY(), YEAR)	Current age in whole years
Students	Full Name	Students[FirstName] & " " & Students[LastName]	First and last name joined
Students	School Stage	SWITCH(TRUE(), Students[Class] <= 8, "Middle School", "High School")	Groups Class 6–8 vs 9–10
Marks	Percentage	DIVIDE(Marks[MarksObtained], Marks[MaxMarks]) * 100	Marks as a % of max marks
Marks	Result	IF(Marks[Percentage] >= 35, "Pass", "Fail")	Pass/Fail per subject per term
Attendance	Attendance %	DIVIDE(Attendance[DaysPresent], Attendance[TotalDays]) * 100	Days present as a % of total days
Fees	Balance Due	Fees[FeeAmount] - Fees[AmountPaid]	Amount still outstanding per term

📐 Part B — DAX Measures
Measure	Formula	What it does
Total Students	DISTINCTCOUNT(Students[StudentID])	Unique student count
Average Percentage	AVERAGE(Marks[Percentage])	Average exam score across all rows
Total Fees Collected	SUM(Fees[AmountPaid])	Total amount actually paid
Total Fees Due	SUM(Fees[Balance Due])	Total amount still outstanding
Fee Collection %	DIVIDE([Total Fees Collected], SUM(Fees[FeeAmount])) * 100	Collected as a % of total billed
Pass Percentage	DIVIDE(CALCULATE(COUNTROWS(Marks), Marks[Result] = "Pass"), COUNTROWS(Marks)) * 100	Share of Marks rows that passed
Average Attendance %	AVERAGE(Attendance[Attendance %])	Average attendance across all months
High Schoolers' Average Percentage	CALCULATE([Average Percentage], Students[School Stage] = "High School")	Average score, High School only

📋 Part C — Calculated Tables
Table	Formula	What it does
Top Performers	FILTER(ADDCOLUMNS(Students, "Avg %", AVERAGEX(RELATEDTABLE(Marks), Marks[Percentage])), [Avg %] >= 90)	Students averaging 90%+ across all their marks
Fee Defaulters	FILTER(Fees, Fees[Balance Due] > 0)	Every fee record with an outstanding balance
Class Summary	SUMMARIZE(Students, Students[Class], "Total Students", COUNTROWS(Students))	One row per class with total enrollment

📈 Dashboard Highlights
KPI cards: Total Students, Average Percentage, Total Fees Collected, Total Fees Due, Fee Collection %, Pass Percentage, Average Attendance %, High Schoolers' Average Percentage
Average Percentage by Subject (bar chart)
Average Attendance % by Month (trend line)
Total Students by Section (pie chart)
Average Percentage by Term (donut chart)

Across the current dataset, the model surfaces a 70.94% average exam score, 99.83% pass rate, 87.02% average attendance, and 85.36% fee collection against total dues.

🛠️ Technology / Tools
Microsoft Power BI Desktop (Get Data, Model view, DAX)
Microsoft Excel (source data)
Data modeling (relationships, one-to-many)
DAX (calculated columns, measures, calculated tables)

🔄 Project Workflow
Greenfield_School_Dataset.xlsx (4 related sheets)
      ↓
Power BI Data Model (relationships on StudentID)
      ↓
Calculated Columns (Age, Full Name, School Stage, Percentage, Result, Attendance %, Balance Due)
      ↓
DAX Measures (totals, averages, % collected, pass rate)
      ↓
Calculated Tables (Top Performers, Fee Defaulters, Class Summary)
      ↓
KPI Dashboard

📁 Project Structure
Greenfield-School-PowerBI-Dashboard/
│
├── JeisaMathew_PowerBI_Greenfield_Assignment.pbix
├── Greenfield_School_Dataset.xlsx
├── Greenfield_School_Dashboard.png
├── Greenfield_School_Dashboard.mp4
├── Power_BI_Data_Modeling_Assignment.docx
└── README.md

📌 Project Deliverables
Power BI file (.pbix) with the full data model, calculated columns, measures, and calculated tables
Dashboard screenshot and video walkthrough
Documentation page listing every formula built, with a one-line explanation of what it does

🔗 Connect
Jeisa Mathew LinkedIn • GitHub
