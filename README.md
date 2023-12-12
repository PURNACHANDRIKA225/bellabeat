# bellabeat
GOOGLE CAPSTONE PROJECT

BELLABEAT CASESTUDY
(Google Data Analytics Capstone Project By Purna Chandrika)
TABLE OF CONTENTS:

1.Company summary

2.ASK phase

3.PREPARE phase

4.PROCESS phase

5.Analyze and Share

6.Conclusions (ACT phase)

1. Company Summary:

BellaBeat is a company that focuses on creating health and wellness technology, with an emphasis on developing products for women. The company was founded by Sandro Mur and Urska Srsen in 2013. BellaBeat gained recognition for its innovative approach to combining technology with fashion, offering wellness trackers designed as stylish jewelry. Products by Bellabeat:

○ Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.

○ Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf tracker connects to the Bellabeat app to track activity, sleep, and stress.

○ Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.

○ Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels. The analysis is done by following the six steps of Analysis :

2.ASK PHASE:

This step involves identifying business tasks, stakeholders and coming up with suitable outcomes, Analyze Bellabeat data to gain insights into user behavior and gather valuable data for future Bellabeat products and marketing. Business Objectives

What are some trends in smart device usage?
How could these trends apply to Bellabeat customers?
How could these trends help influence Bellabeat marketing strategy?
STAKEHOLDERS:

Urška Sršen: Bellabeat’s co-founder and Chief Creative Officer
Sando Mur: Mathematician and Bellabeat’s co-founder; key member of the Bellabeat executive team
Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.
3. PREPARE PHASE: The data source used for this case study is FitBit Fitness Tracker Data. This dataset was downloaded from Kaggle; it is a Public dataset made available through Mobius. Although The FitBit Fitness Tracker Data was obtained in 2016, rendering the datasets out of date for current trend research. Furthermore, while the data initially specifies a time range of 03-12-2016 to 05-12-2016, data verification revealed that the data was obtained only during a 31-day period (04-12-2016 to 05-12-2016). Because the data only covered a 31-day period, the timeframe for a more in-depth examination is actually quite short. In this phase we verified whether the data has ROCC(identifying credibility issues).

Reliable = LOW: the data only contains 30 users
Original = MED: provided by a trusted 3rd-party (Amazon Mechanical Turk)
Comprehensive = MED: provides extensive categories related to fitness
Current = LOW: the data is outdated as it was gathered in 2016
Cited = LOW: unknown
*4. PROCESS PHASE: *

Data sets used during this phase : dailyActivity_merged.csv dailyCalories_merged.csv dailySteps.csv sleepDay_merged.csv weight.csv

Steps followed in this phase:

Exploring data
Check for duplicates & null values
Transform data formats
Query the data
I started off by cleaning the data in Excel, and I followed the following procedures to do so:

Checked and removed duplicates.
Changed the format of date and time and split them into separate columns using “text to columns functions.
Filtered and sorted data by Id and date to find the first and last date of data collection by each Id.
Formatted the numerical data to 2 decimal places.
5. ANALYZE AND SHARE PHASE:

I’ve used SQL to quickly gain valuable insights in the data, Firstly I’ve created a “FITNESS” dataset in BigQuery and uploaded the tables by using the data sets mentioned above.

To quickly get the number of unique user Id in these data sets I’ve runned the following query
add Codeadd Markdown
SELECT count(distinct ID) FROM `bellabeat-407316.FITNESS.daily_activity` LIMIT 1000;
​
add Codeadd Markdown
RESULTS:

Daily_activity: 33

Daily_sleep : 24

Hourly_activity:33

Weight: : 8

Classified the users into Inactive , Active and Very Active based on number of times they’ve logged in the app.
add Codeadd Markdown
 SELECT Id,COUNT(*) as login_Count
FROM `FITNESS.daily_activity`
GROUP BY Id
HAVING COUNT(*) > 1;
add Codeadd Markdown
SELECT Id,
COUNT(Id) AS Total_Logged_Uses,
CASE
WHEN COUNT(Id) BETWEEN 25 AND 31 THEN 'Active User'
WHEN COUNT(Id) BETWEEN 15 and 24 THEN 'Moderate User'
WHEN COUNT(Id) BETWEEN 0 and 14 THEN 'Light User'
END Fitbit_Usage_Type
FROM `FITNESS.daily_activity`
GROUP BY Id;
​
add Codeadd Markdown
RESULTS:

https://docs.google.com/spreadsheets/d/1jRTiv_7ibATt16ttrXVH2vGh_FVzw0imoP0dRDKf1y8/edit?usp=drive_link

Sheet 1 (1).png

add Codeadd Markdown
I’ve further classified them based on the counted average of total steps made by the user.

The classification is done by referring to the https://www.medicalnewstoday.com/ article which stated that Inactivity is defined as fewer than 5,000 steps per day and Average (moderately active): 7,500 to 9,999 steps per day; More than 12,500 steps per day is considered very active.
add Codeadd Markdown
SELECT Id,
round(avg(TotalSteps)) AS Avg_Total_Steps,
CASE
WHEN avg(TotalSteps) < 5000 THEN 'Inactive'
WHEN avg(TotalSteps) BETWEEN 5000 AND 7499 THEN 'Low Active User'
WHEN avg(TotalSteps) BETWEEN 7500 AND 9999 THEN 'Average Active User'
WHEN avg(TotalSteps) BETWEEN 10000 AND 12499 THEN 'Active User'
WHEN avg(TotalSteps) >= 12500 THEN 'Very Active User'
END User_Type
FROM `FITNESS.daily_activity`
GROUP BY Id;
​
add Codeadd Markdown
RESULTS:

https://docs.google.com/spreadsheets/d/1QxIgrtLPPKrJVOJXjS0jAynIq1ANQ-6F-Ca1OujXyr8/edit?usp=drive_link

Sheet 3.png

I ran the following query to determine the relationship between the calories burned by these users and the total distance they’ve covered.
add Codeadd Markdown
SELECT Id, ROUND(avg(TotalDistance)) as avg_total_dist ,ROUND(AVG(Calories)) as avg_calories, FROM `FITNESS.daily_activity` GROUP BY Id;
​
add Codeadd Markdown
RESULTS:

https://docs.google.com/spreadsheets/d/1LJ0LdhY8x85BEhbqhgr7KgBVfzAGFIzsVSKH9_pDKjU/edit#gid=387117712

I ran the following queries to get a better look at these users' sleeping habits and see if they have any effect on their activity level.
To calculate the sum of total sleeping minutes
add Codeadd Markdown
SELECT SleepDay,SUM(TotalMinutesAsleep) AS Total_Minutes_Asleep
FROM `FITNESS.daily_sleep`
WHERE SleepDay IS NOT NULL
GROUP BY SleepDay;
​
add Codeadd Markdown
To Identify the relationship between the sleep time and steps
add Codeadd Markdown
SELECT a.Id,
ROUND(avg(a.TotalSteps)) AS AvgTotalSteps,
ROUND(avg(a.Calories)) AS AvgCalories,
ROUND(avg(s.TotalMinutesAsleep)) AS AvgTotalMinutesAsleep,
FROM `FITNESS.daily_activity` AS a
INNER JOIN `FITNESS.daily_sleep` AS s ON a.Id=s.Id
GROUP BY a.Id;
​
add Codeadd Markdown
Type Markdown and LaTeX:  α2
 
add Codeadd Markdown
RESULTS:

https://docs.google.com/spreadsheets/d/1g0FXpuSLzHM5FDBGfoQRwChnn14kMgx4_Kcbt0wXpAE/edit?usp=drive_link

Sheet 2.png

I ran the following query to determine the relationship between the calories burned by these users and the total distance they’ve covered.
add Codeadd Markdown
​
​
SELECT Id, ROUND(avg(TotalDistance)) as avg_total_dist ,ROUND(AVG(Calories)) as avg_calories, FROM `FITNESS.daily_activity` GROUP BY Id;
​
​
​
add Codeadd Markdown
RESULTS:

https://docs.google.com/spreadsheets/d/1LJ0LdhY8x85BEhbqhgr7KgBVfzAGFIzsVSKH9_pDKjU/edit#gid=387117712

Sheet 5.png

add Codeadd Markdown
6.CONCLUSION:

Push notifications throughout the day for meeting a daily step goal could be implemented, increasing user interaction with technology while also improving health through increased exercise.

Creating a sleep scheduler that keeps track of their sleep schedule and sleep reminders would be beneficial so that active users could get enough sleep to perform well.

For users who want to lose weight, set a daily calorie burn goal; this may encourage them to walk more as a healthy habit.

I'd also suggest sending a daily notification at the end of the day to 'charge up' the wearable so it's ready to track the next day.

By advertising active users' fitness journeys, the company can attract more customers to purchase the products.
