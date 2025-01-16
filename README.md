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
- Data cleaning: Google Sheets
- Data analysis: SQL
- Data visualisation: Tableau

## Step 1 - Data Preparation
After thoroughly reading the case study provided the first step taken was to download and organise the datasets:
- The dataset has been downloaded and stored safely on a drive folder created appositely for this project.

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

## Step 3 & 4 - Data Analysis & Visualisation
### SQL - BigQuery and Tableau

- ### *Organised each usertype by age*
```
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
```
Key Findings:

  Casual customers have registered a high number of NULL (17126): 74% of all values.
  
  This suggests that the company does not require a birth date when a customer rides as a casual.
  
  Implementing this new feature could help the marketing team target a more specific age group to convert casual into members.
  
  On the other hand, we can see that most subscribers are between 25 and 40 years old with peaks between 27 and 32.
  
  However, with the data collected for casuals it shows slightly younger customers with higher number of riders between 24 and 26 years old.

![birthyear](https://github.com/user-attachments/assets/e5e6e487-8f73-4d6d-a1e1-b869bf1005be)

  
- ### *Identify average trip lenght by usertype*
```
SELECT
  ROUND(AVG(CASE WHEN usertype = "Customer" THEN tripduration ELSE NULL END)/60, 0) AS Customer_avg_tripduration_m,
  ROUND(AVG(CASE WHEN usertype = "Subscriber" THEN tripduration ELSE NULL END)/60, 0) AS Subscriber_avg_tripduration_m
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1` ;
```
Key Findings:

  Trip lenght for customers is significantly higher:
  
  Assumption 1. subscribers are more inclined to take more rides and as a consequence the average reduces
  
  Assumption 2. no incentive in taking long trips
  
  Assumption 3. usertypes have different habits, where casuals take more pleasure rides while subscribers tend to use it more to commute to work.

![trip_lenght](https://github.com/user-attachments/assets/a6320611-ba38-4ef5-8a5a-6c415f40835f)


- ### *Count number of trips by usertype*
```
SELECT 
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) AS Customer_trips,
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Subscriber_trips,
  COUNT(CASE WHEN usertype = "Customer" THEN trip_id ELSE NULL END) + 
  COUNT(CASE WHEN usertype = "Subscriber" THEN trip_id ELSE NULL END) AS Total_trips
FROM 
  `cyclistic-project-bike-sharing.Cyclistic_bike_sharing.Trips_2019_Q1` ;
```

- ### *number of trips by each usertype by each day of the week*
```
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
```
Key Findings:

  To consolidate the 3rd assumption in the trip lenght query the data here shows:
  
  Subscribers tend to ride more from MONDAY to FRIDAY while Casuals register a spike in users on SATURDAY and SUNDAY.

![week_day (1)](https://github.com/user-attachments/assets/e1371c12-b570-47e2-a4fa-0e937ccbfe94)


- ### *understand station with most traffic*
```
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
```
This query provide station with the most user traffic

![casuals_more_traffic_station](https://github.com/user-attachments/assets/dc1cff53-28f9-4ccd-93c2-a25652ee4fd5)


## Step 5 - Overall Conclusion and Reccomandations

Strategy n1: Converting Casual customers into Subscribers
- Age target group: 20-30
- Target customers: Leisure, Sports, Tourist
- Focus on longer trip incentives and implement weekend packages to maximise conversion of Casuals into Members.
- Focus on the top 5 stations with the most traffic to implement marketing materials on-site without forgetting about digital tools which can have a wider and bigger reach.

Strategy n2: Understanding Subscribers' behaviours to maximise retention and attract new members
- Age target group: 25-40
- Target customers: City Employees
- Promote sustainability by commuting to work with bikes focusing on office peak time promotion for new users to attract new customers.
- I would suggest having an online, especially on social media, presence to promote green and work values.

## Dashboard

To see my Tableau dashboard [click here](https://public.tableau.com/views/Cyclistic_Project_17370316834640/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
