# ![bellabeat_logo](https://user-images.githubusercontent.com/126125206/226458896-65662259-b843-4722-92a4-f15bf8ffbbc6.png) Case Study: Bellabeat Fitness Data Analysis 
#### Author: Angel Zapata 
#### Date: March 20, 2023
#
**For this case study I followed the six step data analysis process:**
* [Ask](#1-ask)
* [Prepare](#2-prepare) 
* [Process](#3-process)
* [Analyze](#4-analyze)
* [Share](#5-share)
* [Act](#6-act)

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
* daily_sleep.csv
* hourly_steps.csv
* hourly_intensity.csv
* hourly_calories.csv
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

--- Checking for total users for daily_sleep
SELECT COUNT (DISTINCT id) as daily_sleep_total_users
FROM bellabeat.dbo.daily_sleep

--- Checking for total users for heart_rate
SELECT COUNT (DISTINCT id) as heart_rate_total_users
FROM bellabeat.dbo.heart_rate

--- Checking for total users for hourly_calories
SELECT COUNT (DISTINCT id) as hourly_calories_total_users
FROM bellabeat.dbo.hourly_calories

--- Checking for total users for hourly_intensities 
SELECT COUNT (DISTINCT id) as hourly_intensities_total_users
FROM bellabeat.dbo.hourly_intensities

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

## 4. Analyze

#### User Insights
I was interested in finding out how often each user utilized their FitBit devices in the daily_activity dataset. To do this, I ran the following code: 
```
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
* Active Users — wore their tracker for 25–31 days
* Moderate Users — wore their tracker for 15–24 days
* Light Users — wore their tracker for 0 to 14 days

<p align = "center">
	<img src="https://user-images.githubusercontent.com/126125206/228372574-b566038b-3580-495f-b4c3-52d94373d4c6.png" width="500" height="300"/>

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
	<img src="https://user-images.githubusercontent.com/126125206/228710884-5a810cd6-dbb4-4036-b894-ee9eba5b078a.png" width="500" height="300"/>
	
However, these insights led to another question - which of the users are meeting the minimum activity levels recommended by the [CDC](https://www.cdc.gov/physicalactivity/basics/adults/index.htm#:~:text=Each%20week%20adults%20need%20150,Physical%20Activity%20Guidelines%20for%20Americans.&text=We%20know%20150%20minutes%20of,do%20it%20all%20at%20once)?  
The Centers for Disease Control and Prevention recommends 150 minutes of moderate-intensity physical activity. At first glance of the data, it seems unlikely that this requirement will be met. I wrote the following query to add Very Active and Fairly Active activities. 
```
SELECT
AVG(very_active_minutes) + AVG(fairly_active_minutes) AS total_active_minutes
FROM bellabeat.dbo.daily_activity
```
The result came out to be an average of 35 minutes. However, I observed that a considerable number of users recorded 0 minutes in both the Very Active and Fairly Active categories. I have a hypothesis that they didn't use the FitBit device on that particular day, hence I opted to exclude any outcomes that showed a zero value. I decided to run the following query to get results by user: 
```
SELECT id,
ROUND(AVG(very_active_minutes),2) + ROUND(AVG(fairly_active_minutes),2) AS total_active_minutes
FROM bellabeat.dbo.daily_activity
WHERE very_active_minutes <> 0 AND fairly_active_minutes <> 0
GROUP BY id
``` 
Upon my investigation, I found that all users were failing to meet the guidelines set by the CDC. 

```
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


 


## 5. Share


## 6. Act


