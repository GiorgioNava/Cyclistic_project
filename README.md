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

```
-- 1 QUERY Usertype / Birthyear
SELECT
  birthyear,
  2019 - birthyear as age,
  SUM(CASE WHEN usertype = "Customer" THEN 1 ELSE 0 END) AS Customer_count,
  SUM(CASE WHEN usertype = "Subscriber" THEN 1 ELSE 0 END) AS Subscriber_count
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1` 
GROUP BY
  birthyear
ORDER BY 
  birthyear;
-- customer have an high count of "null"
-- probably only mandatory data for subscribers
-- implement strategy to request birth date of customers as well to target specific age group

-- 2 QUERY avg tripduration / usertype
SELECT
  ROUND(AVG(CASE WHEN usertype = "Customer" THEN tripduration ELSE NULL END)/60, 0) AS Customer_avg_tripduration_m,
  ROUND(AVG(CASE WHEN usertype = "Subscriber" THEN tripduration ELSE NULL END)/60, 0) AS Subscriber_avg_tripduration_m
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1` ;
-- tripduration for customers is significantly higher
-- assumption: 1. subscribers are more inclined to take more rides and as a consquence the avg reduces, 2. no incentive in taking long trips

-- 3 QUERY number of trips / usertype
SELECT 
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) AS Customer_trips,
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Subscriber_trips,
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) + 
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Total_trips
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1` ;
-- number of trips of subscribers is significantly higher than customers
-- This suggest high rate of conversion for usual customers

-- 4 QUERY day_of_week / number of trips/ usertype
SELECT
  day_of_week,
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) AS Customer_count,
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Subscriber_count
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1`
GROUP BY
  day_of_week
ORDER BY
  day_of_week;
-- customers tend to ride more in weekends while subscribers in the weekday
-- this can provide good insight to the marketing team to focus on weekends customer conversion

-- 5 QUERY from&to_station_name / trip_id / usertype
SELECT
  from_station_name,
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) AS Customer_count,
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Subscriber_count
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1`
GROUP BY
  from_station_name
ORDER BY
  Customer_count DESC;
SELECT
  to_station_name,
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) AS Customer_count,
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Subscriber_count
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1`
GROUP BY
  to_station_name
ORDER BY
  Customer_count DESC;
-- this query provide insight on where to focus whit marketing material as most traffic of customers is shown
```
