# Cyclistic 
## Bike-share Company

### Data Collection
The data was provided by Google as part of the final Case study of the Data Analytics Certificate.

### Project Scenario
- *Role*: Junior Data Analyst

- *Company*: Bike-share Company

- *Company Location*: Chicago

- *Goal*: Maximize Annual Memberships

  Analyze the differences in how casual riders and annual members use Cyclistic bikes. Provide insights to help develop a marketing strategy to convert casual riders into yearly members.

- *Company Background*: A bike-share program that features 5,824 bicycles and 692 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use the bikes to commute to work each day.

### Data Tools
- Data cleaning: Google Sheets and SQL
- Data analysis: Google Sheets, SQL and R
- Data visualisation: Google Sheets, R and Tableau

## Step 1 - Data Preparation
After thoroughly reading the case study provided the first step taken was to download and organise the datasets:
- The ```Divvy_Trips_2019_Q1``` dataset has been downloaded and stored safely on a drive folder created appositely for this project.

## Step 2 - Data Cleaning
### Google Sheet 
This is the preview of the first 15 rows of the file:

<img width="627" alt="image" src="https://github.com/user-attachments/assets/62225465-07f2-40ad-8494-a459019ca07f" />

Action taken:
- Resized all columns to make the file more clear.
- Converted ```start_time``` and ```end_time``` format to Number > Date and Time.
- Decresead to 0 decimals the ```tripduration``` column.
- Format headers in *BOLD* and with a *FILL COLOR*.
- Freeze 1st row to allow better separation between headers and data values.
- Created a *FILTER*.
- Checked & eliminated duplicated values.

This is how it looked after:

<img width="803" alt="image" src="https://github.com/user-attachments/assets/b72492b4-fac5-4075-b4bf-94e434303509" />


- A ```.csv``` version of the cleaned data has been created and stored in the same manner to be queried in *BIG QUERY* with *SQL*

### SQL - BigQuery


