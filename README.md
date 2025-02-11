# 🎯 SQL + Tableau Data Analysis: Student Career Track Completion Trends
# 📌 Overview
This project analyzes career track completion trends using SQL for data extraction and Tableau for visualization. The goal is to understand student enrollment, completion rates, and time taken to complete career tracks.
🛠️ Technologies Used
SQL (MySQL) – Data extraction & transformation
Tableau – Data visualization
GitHub – Project documentation

# 📊 Steps Involved
# 1️⃣ Extracting Data with SQL
We use a JOIN between career_track_student_enrollments and career_track_info to get student-track pairs. Key transformations:
student_track_id: Unique identifier per student-track pair.
track_completed: 1 if completed, 0 otherwise.
days_for_completion: Number of days taken to complete the track.
completion_bucket: Categorizes completion time into defined ranges.
SQL Query Used:
SELECT
    student_track_id,
    student_id,
    track_name,
    date_enrolled,
    track_completed,
    days_for_completion,
    CASE
        WHEN days_for_completion = 0 THEN 'Same day'
        WHEN days_for_completion BETWEEN 1 AND 7 THEN '1 to 7 days'
        WHEN days_for_completion BETWEEN 8 AND 30 THEN '8 to 30 days'
        WHEN days_for_completion BETWEEN 31 AND 60 THEN '31 to 60 days'
        WHEN days_for_completion BETWEEN 61 AND 90 THEN '61 to 90 days'
        WHEN days_for_completion BETWEEN 91 AND 365 THEN '91 to 365 days'
        WHEN days_for_completion >= 366 THEN '366+ days'
    END AS completion_bucket
FROM
(
    SELECT
        ROW_NUMBER() OVER (ORDER BY student_id, track_name DESC) AS student_track_id,
        e.student_id,
        i.track_name,
        e.date_enrolled,
        e.date_completed,
        CASE WHEN e.date_completed IS NULL THEN 0 ELSE 1 END AS track_completed,
        COALESCE(DATEDIFF(e.date_completed, e.date_enrolled), 0) AS days_for_completion
    FROM
        career_track_student_enrollments e
    JOIN
        career_track_info i ON e.track_id = i.track_id
) a;

# 2️⃣ Creating a Combo Chart in Tableau
A Dual-Axis Chart was created to analyze completion trends:
Bars: Number of students per completion_bucket.
Line: Average days for completion.
Steps:
Drag completion_bucket to Columns.
Drag COUNTD(student_id) to Rows (bar chart for student count).
Drag AVG(days_for_completion) to Rows (line chart for completion time).
Set Dual-Axis and Synchronize Axes.
Format: Blue for bars, Red for line.
Add labels & sorting.

# 3️⃣ Creating a Bar Chart in Tableau
A Bar Chart was created to compare track completion rates.
Steps:
Drag track_name to Columns.
Drag AVG(track_completed) to Rows.
Convert to Percentage Format.
Sort in descending order.
Use Green for high completion rates.

# 📈 Results & Insights
Most students complete tracks within 8 to 30 days.
Some tracks have higher completion rates than others.
Dual-Axis analysis provides better trend visualization.

# 🚀 How to Run This Project
SQL Query Execution: Run the SQL query in MySQL or any compatible database.
Data Import: Load the extracted data into Tableau.
Create Visualizations following the steps outlined above.
# 🏆 Key Takeaways
✅ SQL joins & transformations for data preparation. ✅ Tableau charts for insightful storytelling. ✅ Trend analysis for career track completion.


