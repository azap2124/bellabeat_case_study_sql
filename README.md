# ![bellabeat_logo](https://user-images.githubusercontent.com/126125206/226458896-65662259-b843-4722-92a4-f15bf8ffbbc6.png) Case Study: Bellabeat Fitness Data Analysis 
#### Author: Angel Zapata 
#### Date: March 20, 2023
#

**For this case study I followed the six step data analysis process:**
* [Ask](#1-ask)
* [Prepare](#2-prepare) 
* [Process](#3-process)
* [Analyze & Share](https://github.com/azap2124/bellabeat_case_study/edit/main/README.md#4-analyze--share)
* [Act](#6-act)

Tools used: 
* Microsoft Excel
* SQL Server Management Studio 

## Introduction
Bellabeat is a high-tech manufacturer of health-focused products for women. For this study, I was asked to help their marketing team get insights in order to make informed decisions for the future of their products and marketing strategy. Bellabeat was founded in 2013 and has grown very rapidly. It was quickly positioned as a tech-driven wellness company for women. They have five major products: Bellabeat App, Leaf Wellness Tracker, Time Watch, Spring Water Bottle, and their Bellabeat Membership. 
### About The Company
Bellabeat was founded by Urška Sršen and Sando Mur. Their products collect data on activity, sleep, stress, and reproductive health allowing women to gain knowledge about their own health and habits.  

By 2016, Bellabeat had opened offices around the world and launched multiple products. Bellabeat products became available through a growing number of online retailers in addition to their own e-commerce channel on their website. Sršen and Mur know that an analysis of Bellabeat’s available consumer data would reveal more opportunities for growth. 

### Scenario
Urška Sršen and Sando Murhave have asked the marketing analytics team to focus on a Bellabeat product and analyze smart device usage data in order to gain insight into how people are already using their smart devices. Then, using this information, they would like high-level recommendations for how these trends can inform Bellabeat marketing strategy. I’ll offer my findings and recommendations to Bellabeat’s executive team.  

Urška Sršen: Co-founder and Chief Creative Officer of Bellabeat  

Sando Mur: Mathematician and Bellabeat’s cofounder; a key member of the Bellabeat executive team

## 1. Ask
### Identifying the business task:

I will analyze smart device usage data in order to gain insights into how consumers use non-Bellabeat smart devices. After this is accomplished, I'll select one Bellabeat product and apply these insights to my presentation.

### Questions to consider:
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

**Primary stakeholders:** Urška Sršen and Sando Mur - Executive team members

**Secondary stakeholders:** Bellabeat's marketing analytics team


## 2. Prepare
For this next step, I will use Google's ROCCC methodology in order to figure out if there are any problems with the data and if it has any problems with bias or credibility. I utilized this [database](https://www.kaggle.com/arashnic/fitbit) for my analysis. 

#### Data Source
This dataset includes thirty participants. Thirty Fitbit users willingly provided their consent for the collection of personal tracker data, which included minute-level output for physical activity, heart rate, and sleep monitoring. The dataset has 18 CSV documents each containing different data insights. 

#### ROCCC Approach
* Reliability: **LOW**. A sample size of at least 30 is often considered the minimum acceptable size for statistical analysis. However, considering there's over 31 million active users for Fitbit, this dataset is not really realiable. Also, all of this data was collected in just one month, which is not long enough to find accurate and reliable trends. It's good, however, that the 30 minimum sample size rule is met.
* Originality: **LOW**. The data set was not original and was created through an Amazon Mechanical Turk survey, which is not an ideal approach as it would have been preferable for Fitbit to provide the information directly.
* Comprehensive: **MEDIUM**. In my opinion, this data is missing very important elements to make the analysis more complete. Some information that would be helpful to know is age, height, weight, or sex. We also don't the rules as to how this sample was selected. There is no way to know if the people who were chosen were chosen because of bias or if they were chosen at random. 
* Current: **MEDIUM**. The data collected is outdated, as it was gathered seven years ago, and may not provide an accurate representation of the current situation. However, people's live styles don't change drastically in a of seven year period. 
* Cited: **HIGH**. Data collector and source is well documented. 

## 3. Process
#### Data Selection 
Once I had downloaded the Fitbit dataset and performed file extraction, and after inspecting each file, I proceeded to choose the files that were relevant for my analysis and renamed them using suitable file naming conventions:
* daily_activity.csv
* hourly_steps.csv
* daily_sleep
* hourly_calories.csv
* daily_calories.csv
* heart_rate.csv
* weight_log.csv

#### Cleaning The Data
I chose Excel for data cleaning. The following steps were taken within each dataset:
1. Used the Remove Duplicates tool to get rid of duplicate values. 
2. Formatted the column names to snake case style for consistency throughout my analysis. 
3. Formatted all date data into MM/DD/YY date format.
4. I converted all numerical data into the Number format, with a maximum of two decimal places.

#### Data Integrity
The data that was chosen has been uploaded onto SQL Server Management Studio for the purpose of my analysis. Queries were executed in order to examine the quantity of unique Ids present within each respective table.The following code was ran: 
```
--- Checking for total users for daily_activity
SELECT COUNT (DISTINCT id) as daily_activity_total_users
FROM bellabeat.dbo.daily_activity

--- Checking for total users for heart_rate
SELECT COUNT (DISTINCT id) as heart_rate_total_users
FROM bellabeat.dbo.heart_rate

--- Checking for total users for hourly_calories
SELECT COUNT (DISTINCT id) as hourly_calories_total_users
FROM bellabeat.dbo.hourly_calories

--- Checking for total users for hourly_steps
SELECT COUNT (DISTINCT id) as hourly_steps_total_users
FROM bellabeat.dbo.hourly_steps

--- Checking for total users for weight_log
SELECT COUNT (DISTINCT id) as weight_log_total_users
FROM bellabeat.dbo.weight_log
```
Unfortunately, the heart_rate and weight_log datasets do not include enough data to move forward with analysis (7 and 8 users tracking activity, respectively). I decided to move forward without them.  

Users tracking activity: 
- daily_activity: 33
- daily_sleep: 24
- heart_rate: 7 
- hourly_calories: 33
- hourly_intensities: 33
- hourly_steps: 33
- weight_log: 8

## 4. Analyze & Share

#### User Insights
I was interested in finding out how often each user utilized their FitBit devices in the daily_activity dataset. To do this, I ran the following code: 
```
--- Categorizing users by how much they used their FitBit devices
SELECT id,
COUNT (id) AS total_logged,
	CASE
	WHEN COUNT(id) BETWEEN 25 AND 31 THEN 'Active User'
	WHEN COUNT(id) BETWEEN 15 AND 24 THEN 'Moderate User'
	WHEN COUNT(id) BETWEEN 0 AND 14 THEN 'Light User'
	END user_type
FROM bellabeat.dbo.daily_activity
GROUP BY id
```
```
---Tracking users that strictly wore their devices for the full 31 days
SELECT id, 
total_logged
FROM
(
  SELECT id,
  COUNT(id) AS total_logged
  FROM bellabeat.dbo.daily_activity
  GROUP BY id
) subquery
WHERE total_logged = '31'
```
* Active Users - wore their trackers for 25–31 days
* Moderate Users - wore their trackers for 15–24 days
* Light Users - wore their trackers for 0 to 14 days 
 
**(Only 21 users strictly wore their devices for the 31 days)**

<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/229374829-6de27082-7ebb-45ee-9ad3-129f153b3ba1.png" width="500" height="300"/>

Next, I wrote a query aimed at determining the average duration of user engagement in each level of activity. This was taken from the daily_activity dataset.
```
--- Average activity per user
SELECT
ROUND(AVG(very_active_minutes),1) AS average_very_active,
ROUND(AVG(fairly_active_minutes),1) AS average_fairly_active,
ROUND(AVG(lightly_active_minutes),1) AS average_lightly_active,
ROUND(AVG(sedentary_minutes),1) AS average_sedentary
FROM bellabeat.dbo.daily_activity
````
As anticipated, users on average spent the most number of minutes in the Sedentary Activity level. Given that physically demanding jobs are an exception, it was expected that users would spend the majority of their activity in this level.
	
<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/229374954-7c675b4b-9467-4bc1-b335-c23ff55a7ae8.png" width="500" height="300"/>
	
However, these insights led to another question - which of the users are meeting the minimum activity levels recommended by the [CDC](https://www.cdc.gov/physicalactivity/basics/adults/index.htm#:~:text=Each%20week%20adults%20need%20150,Physical%20Activity%20Guidelines%20for%20Americans.&text=We%20know%20150%20minutes%20of,do%20it%20all%20at%20once)?  
The Centers for Disease Control and Prevention recommends 150 minutes of moderate-intensity physical activity. At first glance of the data, it seems unlikely that this requirement will be met. I wrote the following query to add Very Active and Fairly Active activities. 
```
--- Sum of the averages of very active and fairly active activities
SELECT
AVG(very_active_minutes) + AVG(fairly_active_minutes) AS total_active_minutes
FROM bellabeat.dbo.daily_activity
```
The result came out to be an average of 35 minutes. However, I observed that a considerable number of users recorded 0 minutes in both the Very Active and Fairly Active categories. I have a hypothesis that they didn't use the FitBit device on that particular day, hence I opted to exclude any outcomes that showed a zero value. I decided to run the following query to get results by user: 
```
--- Sum of the averages of very active and fairly active activities excluding 0 minute entries
SELECT id,
ROUND(AVG(very_active_minutes),2) + ROUND(AVG(fairly_active_minutes),2) AS total_active_minutes
FROM bellabeat.dbo.daily_activity
WHERE very_active_minutes <> 0 AND fairly_active_minutes <> 0
GROUP BY id
``` 
Upon my investigation, I found that all users were failing to meet the guidelines set by the CDC. 

```
--- Categorizing users by their activity levels 
SELECT id,
ROUND(AVG(very_active_minutes),2) + ROUND(AVG(fairly_active_minutes),2) AS total_active_minutes,
	CASE
	WHEN ROUND(AVG(very_active_minutes),2) + ROUND(AVG(fairly_active_minutes),2) BETWEEN 0 AND 50 THEN 'Low Activity' 
	WHEN ROUND(AVG(very_active_minutes),2) + ROUND(AVG(fairly_active_minutes),2) BETWEEN 51 AND 100 THEN 'Medium Activity' 
	WHEN ROUND(AVG(very_active_minutes),2) + ROUND(AVG(fairly_active_minutes),2) BETWEEN 101 AND 150 THEN 'High Activity' 
	END activity_level
FROM bellabeat.dbo.daily_activity
WHERE very_active_minutes <> 0 AND fairly_active_minutes <> 0
GROUP BY id
```
<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/229371423-eb8e668e-5d13-44b2-a703-9f94b38a9542.png" width="500" height="300"/>

The category "Low Activity" pertains to individuals who engaged in fairly active and very active activities for a duration of 0 to 50 minutes.
On the other hand, "Medium Activity" pertains to individuals who spent 51 to 100 minutes on fairly active and very active activities.
Lastly, the category "High Activity" includes individuals who spent 101 to 150 minutes engaging in fairly active and very active activities. 
As you can see, only two users were in the High Activity category which is very low.  
	
Next, I wanted to take a sample from a week's worth of data to see how it compared to the whole dataset. I looked at the dates from May 1st to May 7th, 2016.
```
--- Categorizing users by whether or not they met CDC recommendations 
SELECT id, 
SUM(very_active_minutes) + SUM(fairly_active_minutes) AS total_active_minutes,
	CASE 
	WHEN SUM(very_active_minutes) + SUM(fairly_active_minutes) >= 150 THEN 'Meets CDC Recommendation'
	WHEN SUM(very_active_minutes) + SUM(fairly_active_minutes) < 150 THEN 'Does Not Meet CDC Recommendation'
	END CDC_recommendations
FROM bellabeat. dbo. daily_activity
WHERE activity_date BETWEEN '5/1/2016' AND '5/7/2016'
GROUP BY id
```
<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/229373615-0c7f81cc-e741-4dbd-803f-2bdf6a018c8b.png" width="500" height="300"/>	

The results came out as follow: 
- Users who do not meet the CDC recommendations: 9 
- Users who meet CDC recommendations: 21

#### Average Steps by Users	
The Mayo Clinic, a non-profit organization dedicated to clinical practice, education, and research, published [**this article**](https://www.mayoclinic.org/healthy-lifestyle/fitness/in-depth/10000-steps/art-20317391#:~:text=The%20average%20American%20walks%203%2C000,a%20day%20every%20two%20weeks.) suggesting that individuals should aim to take 10,000 steps per day. I was interested in finding out which users were achieving the recommended daily goal of taking 10,000 steps per day.
	
According the the article, the average person walks walks 3,000 to 4,000 steps a day. To classify the users even further, I created the following groupings: 
* Passive users: 0 - 4,500 steps 
* Average users: 5,501 - 9,500 steps 
* Very active: 9,500+ steps 
Here's my code: 
```
--- Average steps per user
SELECT id,
AVG(total_steps) AS average_steps,
CASE
	WHEN ROUND(AVG(total_steps),0) < 4500 THEN 'Passive user'
	WHEN ROUND(AVG(total_steps),0) BETWEEN 4501 AND 9999 THEN 'Average user'
	WHEN ROUND(AVG(total_steps),0) > 10000 THEN 'Very active user'
END user_type
FROM bellabeat.dbo.daily_activity
GROUP BY id
```

<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/230232543-4580f7e8-c3ac-40d0-9cc0-756d85bea66c.png" width="500" height="300"/>  

Here are my results: 
* Passive users: 6
* Average users: 20
* Very active: 7  

Out of all the users, 79% didn't meet the goal of 10,000 steps per day. Of these users, 61% were moderately active, while 18% were not very active.

#### Total Steps by the Hour
Next, my goal was to examine the user's total steps by the hour in order to determine the peak hours of activity for our users. Here's my code: 
```
--- Steps by the hour
SELECT 
activity_hour,
SUM(total_steps) AS total_steps_by_hour
FROM bellabeat.dbo.hourly_steps
GROUP BY activity_hour
ORDER BY activity_hour 
```
The top 5 hours were: 
1. 6:00 pm — 542,848 steps
2. 7:00 pm — 528,552 steps
3. 12:00 pm — 505,848 steps
4. 5:00 pm — 498,511 steps
5. 2:00 pm — 497,813 steps
	
We can say that the busiest hours were in the afternoon (from 12:00pm to 2:00pm) and  evening hours (5:00pm to 7:00pm). 

<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/230245709-5738f3c6-7d7f-4e24-9975-4b336611f14b.png" width="500" height="300"/>   
	
#### Calories Per Day of the Week
For the daily_calories file I used the TEXT (=TEXT(A1,"dddd")) function in Excel to change dates into days of the week. I then wrote a query in SSMS to determine the average calories burned based on the day of the week to see if it would give me noticeable insights. 
```
--- Averaging calories by day of the week 
SELECT 
day_of_week,
AVG(calories) AS total_calories
FROM bellabeat.dbo.daily_calories
GROUP BY day_of_week
ORDER BY day_of_week ASC
```
<p align = "center">	
	<img src="https://user-images.githubusercontent.com/126125206/230256467-d4b85752-f0d0-4920-8d8a-f5d4621bdf36.png" width="500" height="300"/>  

For some reason, Thursdays and Sundays had the lowest calories burnt averages. It could be a result of the type of activities people typically engage in on those days. For example, people might be more likely to engage in sedentary activities like watching TV or spending time with family on Sundays, leading to fewer calories burnt. It could also be because those days are closer to the weekend. 
#### Tracking Sleep
I was interested to see how many users were meeting the 7 - 9 hours of daily sleep. I ran the following query: 
```
--- Tracking sleep 
SELECT id, 
sleep_quality
FROM
(
SELECT id,
AVG(total_minutes_asleep/60) AS average_sleep,
CASE 
	WHEN AVG(total_minutes_asleep/60) < 7 THEN 'No sleep'
	WHEN AVG(total_minutes_asleep/60) > 7 THEN 'Enough sleep'
END sleep_quality
FROM bellabeat.dbo.daily_sleep
GROUP BY id
)subquery
WHERE sleep_quality = 'No sleep'
```
Results are as follows: 13 users on average were not getting enough daily sleep. **That's 41% of users.**

## 6. Act
The objective of the business task was to analyze data on the usage of non-Bellabeat products, in order to gain valuable insights into consumer trends in the smart device industry. These insights would then be applied to Bellabeat's customers and future marketing strategies, optimizing revenues and growth for the company while leveraging Bellabeat's growing user base in the smart device and tech-wellness sector. This will involve implementing the findings in the Bellabeat App and upcoming products.

#### Trends Identified
* Some of the participants didn't keep up with logging or tracking their data regularly, and only a small number of them recorded their sleep or weight measurements. Out of all the participants, only 24 users logged their sleep data, and only 8 users logged their weight data.
* Participants spent **81%** on average engaging in sedentary activities.  
* Users failed to wear their devices consistently for the 31 days. **Only 63%** wore their devices daily. 
* On a weekly basis, **30%** of users were not meeting the CDC recommendations for daily activity. On average, 31 users failed to meet CDC recommendations. That's **94%** of our users. 
* Out of all the users, **79%** did not meet the goal of 10,000 steps per day. Of these users, **61%** were moderately active, while **18%** were not very active. The afternoon (from 12:00pm to 2:00pm) and evening hours (5:00pm to 7:00pm) had the most steps tracked during the day. 
* Thursdays and Sundays had the lowest calories burnt on average. 
* On average, a significant **41%** of users were failing to get the recommended 7 - 9 hours of sleep. 





