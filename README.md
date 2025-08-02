# project2-Nutrition-Paradox
Project Title
⚖️ Nutrition Paradox: A Global View on Obesity and Malnutrition
Skills take away From This Project
Public Dataset Exploration
ETL
Data Cleaning and Feature Engineering
Exploratory Data Analysis(EDA)
SQL Table Creation and Data Insertion
Query Writing (Single and Join Queries)
Power BI Visualizations with SQL Backend (or) Streamlit
Domain
Global Health & Nutrition Analytics

📣 Problem Statement
Imagine you are a Data Analyst working for a global health organization. Your task is to investigate the complex challenge of undernutrition and overnutrition across different countries, age groups, and genders. You will use publicly available WHO data to uncover trends, patterns, and disparities in obesity and malnutrition rates around the world. Your end goal is to derive meaningful insights that can inform global health strategies and help tackle the nutritional paradox of rising obesity and persistent malnutrition.


Business Use Cases
Nutrition Risk Monitoring: Identify countries with extremely high obesity or malnutrition levels, flagging them for public health intervention.
Demographic Disparity Analysis: Understand how gender and age groups contribute differently to obesity and malnutrition statistics.
Data-Driven Policy Planning: Help policymakers prioritize regions for funding and nutrition-related programs based on multi-dimensional data.
Comparative Region Analysis: Enable researchers to compare regional trends and draw correlations between health indicators and socio-economic conditions.
Public Health Reporting: Provide government or NGOs with a summarized SQL + Power BI dashboard showing country-level nutrition trends.


📚Project Approach
Step 1:Dataset Overview & Collection
4 public WHO API URLs  each representing a different nutritional indicator have been provided:
For Obesity:
https://ghoapi.azureedge.net/api/NCD_BMI_30C – Obesity among adults (BMI ≥ 30)
https://ghoapi.azureedge.net/api/NCD_BMI_PLUS2C – Obesity/Overweight among children 

For Malnutrition:
https://ghoapi.azureedge.net/api/NCD_BMI_18C – Underweight in adults (BMI < 18.5)
https://ghoapi.azureedge.net/api/NCD_BMI_MINUS2C – Thinness in children
Each dataset provides estimates by country, sex, year, and region, along with confidence intervals (upper and lower bounds).
Preprocessing steps:
Load all 4 datasets.
Add a new column age_group to distinguish adults and children.
Combine the two obesity datasets into one dataframe called df_obesity.
Combine the two malnutrition datasets into one dataframe called df_malnutrition.
Filter each dataset to include only records from the years 2012 to 2022

Step 2: 🧹 Data Cleaning & Feature Engineering
Columns to Retain: 
Only keep the following columns from each dataset:
ParentLocation 
Dim1 
TimeDim 
Low 
High 
NumericValue
SpatialDim 
age_group

 Rename Columns for Consistency:
TimeDim → Year
Dim1 → Gender
NumericValue → Mean_Estimate
Low → LowerBound
High → UpperBound
ParentLocation → Region
SpatialDim → Country
In the Gender column, standardize values to 'Male', 'Female', and 'Both'

Convert Country Codes to Full Names using pycountry:
Use the pycountry Python package to convert 3-letter country codes (ISO Alpha-3) into full country names in the Country column.Reference
🔽 Step-by-step instructions:
Install ‘pycountry’ package 
Import the package in your Python script or notebook
Define a function to convert the 3-letter codes to full names
Apply the function to the Country colum
Handle any remaining unmatched codes using the following dictionary for special values like 'GLOBAL', 'AFR', 'SEAR', etc.
special_cases = {
                    'GLOBAL': 'Global',
                    'WB_LMI': 'Low & Middle Income',
                    'WB_HI': 'High Income',
                    'WB_LI': 'Low Income',
                    'EMR': 'Eastern Mediterranean Region',
                    'EUR': 'Europe',
                    'AFR': 'Africa',
                    'SEAR': 'South-East Asia Region',
                    'WPR': 'Western Pacific Region',
                    'AMR': 'Americas Region',
                    'WB_UMI': 'Upper Middle Income'}


	New Columns to Create:
age_group: Manually assign this column based on the dataset source.
Use "Adult" for datasets NCD_BMI_30C and NCD_BMI_18C.
Use "Child/Adolescent" for datasets NCD_BMI_PLUS2C and NCD_BMI_MINUS2C.
CI_Width: Calculate the confidence interval width.
Formula: CI_Width = High - Low
obesity_level (for the obesity table only):
Categorize obesity levels based on NumericValue:
>= 30 → High
25–29.9 → Moderate
< 25 → Low
malnutrition_level (for the malnutrition table only):
Categorize malnutrition levels based on NumericValue:
>= 20 → High
10–19.9 → Moderate
< 10 → Low
   Each dataset will end up with 10 columns after cleaning and feature engineering

Column Descriptions
S.no
Columns
Description
1
Year
Year of Data collection
2
Gender
Sex(Male,Female,Both)
3
Mean_Estimate
Estimated percentage of obesity or malnutrition
4
LowerBound
Lower bound of confidence Interval
5
UpperBound
Upper bound of confidence interval
6
Age_Group
'Adult' or 'Child/Adolescent' based on dataset
7
Country
Country name derived from 3-letter code 
8
Region
WHO region(e.g. Africa, Europe)
9
CI_Width
Confidence interval range (High - Low)
10
Obesity_level
Obesity category derived from NumericValue (only in obesity table)
11
Malnutrition_Level
Malnutrition category derived from NumericValue (only in malnutrition table)


Step:3 🧮 Exploratory Data Analysis (EDA) with Python
Perform basic Exploratory Data Analysis (EDA) on the cleaned datasets using Python.
Key EDA tasks may include:
Understanding the shape and structure of the data
Identifying missing values or unusual values
Checking distribution of Mean_Estimate, CI_Width, etc.
Analyzing trends across years, regions, or demographic groups
Comparing obesity vs malnutrition distributions
Try different visualizations using Matplotlib or Seaborn or Plotly to uncover interesting patterns 
Some visualization ideas:
Line plots to explore trends over time
Bar charts to compare top/bottom countries or regions
Box plots to observe variability by region
Heatmaps or scatter plots to identify patterns and outliers
Learners are encouraged to explore the data creatively and share any interesting findings or questions that emerge from the visual analysis.

Step 4: Insert Data into SQL
✅ SQL Insertion Steps (via Python)
Create a database


Use any SQL engine like MySQL, PostgreSQL, or SQLite


For MySQL/PostgreSQL: create manually in GUI or CLI


For SQLite: a .db file is auto-created


Establish a connection using Python


Use mysql.connector or pymysql for MySQL


Use psycopg2 for PostgreSQL


Use sqlite3 for SQLite


Create a cursor object


Used to execute SQL commands from Python


Create 2 tables using Python code


obesity 
malnutrition
      5.  Insert Cleaned Data
Insert rows from your final DataFrames (df_obesity and df_malnutrition) into the respective tables using .iterrows() in a for loop.


🔢 Step 4: Query Writing(PowerBI/Streamlit)
🧋 Obesity Table (10 Queries)
Top 5 regions with the highest average obesity levels in the most recent year(2022)
Top 5 countries with highest obesity estimates
Obesity trend in India over the years(Mean_estimate)
Average obesity by gender
Country count by obesity level category and age group
Top 5 countries least reliable countries(with highest CI_Width) and Top 5 most consistent countries (smallest average CI_Width)
Average obesity by age group
Top 10 Countries with consistent low obesity (low average + low CI)over the years
Countries where female obesity exceeds male by large margin (same       year)
Global average obesity percentage per year

👾 Malnutrition Table (10 Queries)
Avg. malnutrition by age group
Top 5 countries with highest malnutrition(mean_estimate)
Malnutrition trend in African region over the years
Gender-based average malnutrition
Malnutrition level-wise (average CI_Width by age group)
Yearly malnutrition change in specific countries(India, Nigeria, Brazil)
Regions with lowest malnutrition averages
8.       Countries with increasing malnutrition (💡 Hint: Use MIN() and MAX()   on Mean_Estimate per country to compare early vs. recent malnutrition levels, and filter where the difference is positive using HAVING.)
9.       Min/Max malnutrition levels year-wise comparison
10.      High CI_Width flags for monitoring(CI_width > 5)

🔗 Combined (5 Queries)
Obesity vs malnutrition comparison by country(any 5 countries)
Gender-based disparity in both obesity and malnutrition
Region-wise avg estimates side-by-side(Africa and America)
Countries with obesity up & malnutrition down
Age-wise trend analysis

📊 Step 5: Power BI Visualization Tasks(Optional)
Connect SQL database to Power BI, load both the obesity_data and malnutrition_data tables, and use them to develop an interactive dashboard.
	Requirements:
Create a Power BI report using the two tables.
Design at least 10-15 visualizations based on the SQL queries.
Combine insights from both datasets using filters, slicers, and visual elements.
Present trends, comparisons, and anomalies using charts like maps, bar charts, line charts, scatter plots, etc.
The goal is to translate analytical results into easy-to-understand, compelling visuals.
	Below are 10 sample visualization ideas:(optional)
Line chart: Global obesity & malnutrition trends
Bar chart: Top 10 countries by obesity vs malnutrition
Map visual: Highlight regions based on obesity levels
Stacked bar: Obesity and malnutrition by gender
Pie chart: Country count by obesity/malnutrition level
Heatmap: CI_Width distribution by region
Dual-line: Obesity vs malnutrition trend in a country
Scatter plot: Obesity vs malnutrition per country
Treemap: Obesity burden per region
Decomposition tree: Drill down by Region → Country → Age Group → Gender

📌 Note for Learners:
 You are required to display the outputs of your SQL queries and EDA either in Streamlit or PowerBI
If using Streamlit, connect your SQL database to your app, run your queries(all 25), and show the results as interactive tables and EDA visualizations within the app.
If using Power BI, you can upload your SQL queries(any 10 or 15) through the Get Data > SQL Server option or by connecting to your local database, and then visualize the query outputs using Power BI charts, tables, and dashboards and try additional 10 visualizations(custom). And display the output of all the 25 query outputs in a Python notebook.

🧾 Step 6: Project Presentation guidelines (Summary & Insight Generation)
While presenting the project , learners are expected to:
Develop a summary of their entire analysis, covering how they cleaned, prepared, analyzed, and visualized the data.
Derive key insights based on SQL query results and the visualizations created using Python or Power BI.
Provide valuable suggestions or recommendations, such as:
Which regions require urgent intervention?
Are there noticeable trends in obesity or malnutrition over time?
Which demographic groups are most vulnerable?
How reliable is the data across regions (based on CI_Width)?
What health strategies could be informed by these findings?
These conclusions should help demonstrate how data can guide public health actions.

🛠 Technical Tags
Python, Pandas, SQL (SQLite / MySQL / PostgreSQL), Data Cleaning, Feature Engineering, Data Visualization, Matplotlib / Seaborn / Plotly, Power BI, pycountry,Public Health Data

📊 Expected Results
Two clean, structured dataframes and SQL tables
25 SQL queries (python notebook)
EDA 
Visualizations using python visualization libraries (7 to 12)
Comprehensive Power BI Dashboards(10 visualizations based on the SQL queries and 10 additional custom visualizations designed by the learner) or Streamlit UI to display the query and EDA visualization outputs.
Summary of the analysis (the coexistence of obesity and malnutrition globally)
