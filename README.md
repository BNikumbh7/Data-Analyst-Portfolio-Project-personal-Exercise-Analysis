# Data-Analyst-Portfolio-Project-personal-Exercise-Analysis
As a exercise enthusiast Exercise enthusiast I want to track my steps so that to ensure I get enough activity.
A Power BI dashboard which lets me get an easy and quick overview of my exercise data.

# Data Collection & Table Structures
The necessary data were collected and structured in Excel files. The exercise data was organized as a fact table and date & activity were organized as dimension tables for filtering data.

# FACT_ Exercise
Exercise data were recorded every single date and the focus was on amount of steps. The date column is used to connect to the date dimension and Activity_FK is used to connect to the activity dimension.
Following steps were done in Power BI to transform this table to be ready for analysis purposes:
Promoted row so that the data so that the first row was used as headers.
Removed unnecessary columns.
Changed column to have the correct type (date, numbers etc.) for later use in calculations.

# DIM_Activity
DIM_Activity describes two different types of activities: Walking and running. On some days walking was chosen as a preferred activity time, and other days running were performed. Some minor data issues in this table was later cleaned up and corrected.
Following steps were done in Power BI to transform this table to be ready for analysis purposes:
Promoted row so that the data so that the first row was used.
Renamed necessary columns to give better business friendly names.
Capitalized each for in the description column for improved data quality.

# DIM_Date
The DIM_Date dimension is based on a simple table with dates, where date was used to derive several new fields which would be used in the exercise analysis dashboard:
Promoted row so that the data so that the first row was used.
Changed the column to DateType
Inserted several new columns based on the date.

# Data Model
the data model after cleansed and prepared tables were read into Power BI.
the FACT table is connected to two dimension tables with the correct relationship established (1 to *) between dimension and fact tables.

# Calculations
The following calculations were created in the Power BI reports using DAX (Data Analysis Expressions). To lessen the extent of coding, the re-use of measures (measure branching) was emphasized:

# Average Steps – This is a simple AVERAGE function around the Steps column:
AVERAGE( FACT_Activity[Steps] )

# Total Steps – This is a simple SUM function around the Steps column:
SUM( FACT_Activity[Steps] )

# Steps (Running) – This is a calculation to isolate the Total Steps measure by filtering it by the “Running Activity”:

CALCULATE(
[Total Steps],
DIM_Activity[ActivityName] = “Running”
)

# Steps (Walking) – This is a calculation to isolate the Total Steps measure by filtering it by the “Walking Activity”:

CALCULATE(
[Total Steps],
DIM_Activity[ActivityName] = “Walking”
)

# Running % of Total – Here we are using two measures from before to find the % of steps that were done by running:

DIVIDE(
[Steps (Running)],
[Total Steps]
)

# Walking % of Total – Here we are using two measures from before to find the % of steps that were done by walking:

DIVIDE(
[Steps (Walking)],
[Total Steps]
)

# Total Steps (Cumulative) – Here we are re-using the Total Steps measure and using different functions to cumulatively calculate the total steps:

CALCULATE(
[Total Steps],
FILTER(
ALLSELECTED( DIM_Date ),
DIM_Date[Date]
<= MAX( FACT_Activity[Date] )
)
)

# Week Over Week % Change Steps – Here we are using the Total Steps measure and using different functions, with variables, to calculate the Week over Week % Change of Steps:

VAR CurrentWeek =
CALCULATE(
[Total Steps],
FILTER(
ALL( DIM_Date ),
DIM_Date[Week of Year]
= SELECTEDVALUE( DIM_Date[Week of Year] )
)
)
VAR PreviousWeek =
CALCULATE(
[Total Steps],
FILTER(
ALL( DIM_Date ),
DIM_Date[Week of Year]
= SELECTEDVALUE( DIM_Date[Week of Year] ) – 1
)
)
RETURN
DIVIDE(
( CurrentWeek – PreviousWeek ),
PreviousWeek
)

