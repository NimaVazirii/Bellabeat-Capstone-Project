# Bellabeat-Capstone-Project
<img width="381" alt="Screen Shot 2022-07-01 at 7 13 03 PM" src="https://user-images.githubusercontent.com/108308205/176982981-5149a1b2-59c6-40fb-8403-cf757d4b0208.png">

Author: Nima Vaziri

Date: 1 July 2022

This Case Study from Google Analytics Course provides me the opportunity to showcase my Technical Skills in SQL, Tableau, and R Programming.

## Introduction

Bellabeat is a high-tech manufacturer of beautifully-designed health-focused smart products for women since 2013. Inspiring and empowering women with knowledge about their own health and habits, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for females.

The Co-founder and Chief Creative Officer, Urška Sršen is confident that an analysis of non-Bellabeat consumer data (ie.FitBit fitness tracker usage data) would reveal more opportunities for growth.


## Business Task

Analyze FitBit Fitness Tracker App data to gain insights into how consumers are using the app and help guide Marketing Strategy for BellaBeat to grow as a Global organization in Tech industry.

## Stakeholders

Primary Stakeholders:
* Urška Sršen: Bellabeat's Cofounder and Chief Creative Officer.
* Sando Mur: Mathematician and another Bellabeat's Cofounder, key member of the Bellabeat executive team.

Secondary Stakeholders:
* Bellabeat Marketing Analytics Team:  A team of data anlysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat's marketing strategy.

## Prepare

* First, we have to understand how data is generated and collected in this phase of analyis: Our Data Source is [Kaggle](https://www.kaggle.com/datasets/arashnic/fitbit). Please refer to this [link](https://www.fitabase.com/media/1930/fitabasedatadictionary102320.pdf) for documentation of the dataset.
* Second, we need to make sure that data is unbiased and credibe: Our dataset contains 18 CSV files and data is from 30 FitBit users who consented to the submission of personal tracker data via Amazon Mechanical Turk. Although this dataset is reliable, original, and comprehensive, there are two main concenrs about it. Firstly, the sample size is small and most data is recorded during certain days of the week. Secondly, Data is outdated for 6 years as the study was conducted in 2016. Also, demographic information such as gender was not included, which is critical to this project as Bellabeat's product line caters for women. 

## Process

The tools that were used in the Process phase of this project are Google's BigQuery and Microsoft Excel. Pre-cleaning of the data was done through Microsoft Excel before the data were stored to BigQuery. 
- By going through all 18 CSV files, I decided to structure my analysis based on *DailyActivity*, *SleepDay*, *MinuteMETs*, *HourlyCalories*, *Hourlyintensities*, and *HourlySteps* tables.
- *Dailyintensities*, *DailyCalories*, *DailySteps*, *MinuteCalories*, *MinuteIntensities*, and *MinuteSteps* were not used in this project since all data points from these tables were already included in the *DailyActivity* and *Hourly* tables.
- Although *HearRate* and *WeightLogInfo* tables would have been useful for this anaysis, there were not utilized due to lack of participants. 
- While uploading the selected datasets to BigQuery, an error occured due to the format of datetime columns. 
-  To fix the *Formatting* issue, all the above Datasets converted into Microsoft Excel in order to create new datetime columns with *mm/dd/yyyy hh:mm AM/PM Formt*. 
- Loaded cleaned datasets into new sheets, saved as CSV files, and uploaded into Bigquery.

After uploading the cleaned Datasets to BigQuery, the following were done to ensure that the data is clean before proceeding to the *Analyze Phase*.

```sql
-- View and examine tables

-- 940 records from DailyActivity table
SELECT *
FROM `expanded-bebop-352104.fitbit.DailyActivity` AS DailyActivity


-- 413 records from SleepDay table
SELECT *
FROM `expanded-bebop-352104.fitbit.SleepDay` AS SleepDay

-- 22,099 records from HourlyCalories table
SELECT *
FROM `expanded-bebop-352104.fitbit.HourlyCalories` AS HourlyCalories

-- 22,099 records from HourlyIntensities table
SELECT *
FROM `expanded-bebop-352104.fitbit.HourlyIntensities` AS HourlyIntensities

-- 22,099 records from HourlySteps table
SELECT *
FROM `expanded-bebop-352104.fitbit.HourlySteps` AS HourlySteps

-- 1,325,580 records from MinuteMETs table
SELECT *
FROM `expanded-bebop-352104.fitbit.MinuteMETs` AS MinuteMETS

-- Number of Unique Participants

-- 33 unique participants in DailyActivity table
SELECT COUNT(DISTINCT id) AS UserCount
FROM `expanded-bebop-352104.fitbit.DailyActivity`

-- 33 unique participants in METs table
SELECT COUNT(DISTINCT id) AS UserCount
FROM `expanded-bebop-352104.fitbit.MinuteMETs` AS MinuteMETS

-- 24 unique participants in SleepDay table
SELECT COUNT(DISTINCT id) AS UserCount
FROM `expanded-bebop-352104.fitbit.SleepDay` AS SleepDay

-- 33 unique participants in HourlyCalories, HourlyIntensities, and HourlySteps
SELECT COUNT(DISTINCT id) AS UserCount
FROM `expanded-bebop-352104.fitbit.HourlyCalories` AS HourlyCalories

SELECT COUNT(DISTINCT id) AS UserCount
FROM `expanded-bebop-352104.fitbit.HourlyIntensities` AS HourlyIntensities

SELECT COUNT(DISTINCT id) AS UserCount
FROM `expanded-bebop-352104.fitbit.HourlySteps` AS HourlySteps

-- No duplicates found on DailyActivities table
SELECT Id, 
  ActivityDate, 
  TotalSteps, 
  TotalDistance, 
  TrackerDistance, 
  LoggedActivitiesDistance,	
  VeryActiveDistance,	
  ModeratelyActiveDistance,	
  LightActiveDistance,	
  SedentaryActiveDistance,	
  VeryActiveMinutes,	
  FairlyActiveMinutes,	
  LightlyActiveMinutes,	
  SedentaryMinutes,	
  Calories,
  COUNT(*) AS Dupes
FROM `gda-course-4-332812.Capstone.DailyActivity` AS DailyActivity
GROUP BY Id, 
  ActivityDate, 
  TotalSteps, 
  TotalDistance, 
  TrackerDistance, 
  LoggedActivitiesDistance,	
  VeryActiveDistance,	
  ModeratelyActiveDistance,	
  LightActiveDistance,	
  SedentaryActiveDistance,	
  VeryActiveMinutes,	
  FairlyActiveMinutes,	
  LightlyActiveMinutes,	
  SedentaryMinutes,	
  Calories
HAVING Dupes > 1

-- Found 3 duplicates from SleepDay table, removed those duplicates.
SELECT
  Id,
  SleepDay,
  TotalSleepRecords,
  TotalMinutesAsleep,
  TotalTimeInBed,
  COUNT(*) AS Dupes
FROM `gda-course-4-332812.Capstone.SleepDay` AS SleepDay1
GROUP BY Id,
  SleepDay,
  TotalSleepRecords,
  TotalMinutesAsleep,
  TotalTimeInBed
HAVING Dupes = 1
ORDER BY Id,
  SleepDay,
  TotalSleepRecords,
  TotalMinutesAsleep,
  TotalTimeInBed

-- Created user no table to clearly identify unique participant
-- Merged Daily Activity, Sleep Day, and User No tables.
-- Nulls have been set to 0.
SELECT 
  DailyActivity.Id,
  UserNumberTable.UserNo,
  ActivityDate, 
  SUM(TotalSteps) AS Total_Steps, 
  SUM(TotalDistance) AS Total_Distance, 
  SUM(TrackerDistance) AS Total_Tracker_Distance, 
  SUM(LoggedActivitiesDistance) AS Total_LoggedActivitiesDistance,	
  SUM(VeryActiveDistance) AS Total_VeryActiveDistance,	
  SUM(ModeratelyActiveDistance) AS Total_ModeratelyActiveDistance,	
  SUM(LightActiveDistance) AS Total_LightActiveDistance,	
  SUM(SedentaryActiveDistance) AS Total_SedentaryActiveDistance,	
  SUM(VeryActiveMinutes) AS Total_VeryActiveMinutes,
  SUM(FairlyActiveMinutes) AS Total_FairlyActiveMinutes,
  SUM(LightlyActiveMinutes) AS Total_LightlyActiveMinutes,
  SUM(SedentaryMinutes) AS Total_SedentaryMinutes,
  SUM(Calories) AS Total_Calories,
  IFNULL(SUM(TotalSleepRecords),0) AS Total_SleepRecords,
  IFNULL(SUM(TotalMinutesAsleep),0) AS Total_MinutesAsleep,
  IFNULL(SUM(TotalTimeInBed),0) AS Total_TimeInBed,
FROM `gda-course-4-332812.Capstone.DailyActivity` AS DailyActivity
LEFT JOIN 
(SELECT
  Id,
  SleepDay,
  TotalSleepRecords,
  TotalMinutesAsleep,
  TotalTimeInBed,
  COUNT(*) AS Dupes
FROM `gda-course-4-332812.Capstone.SleepDay` AS SleepDay1
GROUP BY Id,
  SleepDay,
  TotalSleepRecords,
  TotalMinutesAsleep,
  TotalTimeInBed
HAVING Dupes = 1
ORDER BY Id,
  SleepDay,
  TotalSleepRecords,
  TotalMinutesAsleep,
  TotalTimeInBed) AS SleepDay1
ON DailyActivity.Id = SleepDay1.Id
AND DailyActivity.ActivityDate = SleepDay1.SleepDay
LEFT JOIN `gda-course-4-332812.Capstone.UserNumberTable` AS UserNumberTable
ON DailyActivity.Id = UserNumberTable.Id
GROUP BY DailyActivity.Id, UserNumberTable.UserNo, DailyActivity.ActivityDate
ORDER BY DailyActivity.Id, UserNumberTable.UserNo, DailyActivity.ActivityDate

--Hourly intensity data, extracted year, month, day, day name, hour
-- No duplicates found
SELECT HourlyIntensities.Id, HourlyIntensities.ActivityHour, SUM(TotalIntensity) AS Total_Intensity, AVG(AverageIntensity) AS Avg_Intensity, 
    EXTRACT(YEAR FROM HourlyIntensities.ActivityHour) AS Year,
    EXTRACT(MONTH FROM HourlyIntensities.ActivityHour) AS Month,
    EXTRACT(DAY FROM HourlyIntensities.ActivityHour) AS Day,
    EXTRACT(DAYOFWEEK FROM HourlyIntensities.ActivityHour) AS DayName,
    EXTRACT(Hour FROM HourlyIntensities.ActivityHour) AS Hour,
    COUNT(*) AS Duplicates
FROM `gda-course-4-332812.Capstone.HourlyIntensities` AS HourlyIntensities
GROUP BY HourlyIntensities.Id, HourlyIntensities.ActivityHour
ORDER BY HourlyIntensities.Id, HourlyIntensities.ActivityHour

--Hourly calories data, extracted year, month, day, day name, hour
-- No duplicates found
SELECT Id, ActivityHour, SUM(Calories) AS Total_Calories,
  EXTRACT(YEAR FROM ActivityHour) AS Year,
  EXTRACT(MONTH FROM ActivityHour) AS Month,
  EXTRACT(DAY FROM ActivityHour) AS Day,
  EXTRACT(DAYOFWEEK FROM ActivityHour) AS DayName,
  EXTRACT(Hour FROM ActivityHour) AS Hour,
  COUNT(*) AS Duplicates
FROM `gda-course-4-332812.Capstone.HourlyCalories` AS Hourly_Calories
GROUP BY Id, ActivityHour
ORDER BY Id, ActivityHour

--Hourly steps data, extracted year, month, day, day name, hour
-- No duplicates found
SELECT Id, ActivityHour, SUM(StepTotal) AS Total_Steps,
  EXTRACT(YEAR FROM ActivityHour) AS Year,
  EXTRACT(MONTH FROM ActivityHour) AS Month,
  EXTRACT(DAY FROM ActivityHour) AS Day,
  EXTRACT(DAYOFWEEK FROM ActivityHour) AS DayName,
  EXTRACT(Hour FROM ActivityHour) AS Hour,
  COUNT(*) AS Duplicates
FROM `gda-course-4-332812.Capstone.HourlySteps` AS HourlySteps
GROUP BY Id, ActivityHour
ORDER BY Id, ActivityHour

--Merged 3 hourly tables into one
SELECT HourlyIntensities.Id, HourlyIntensities.ActivityHour, SUM(TotalIntensity) AS Total_Intensity, AVG(AverageIntensity) AS Avg_Intensity, 
  SUM(HourlyCalories.Total_Calories) AS TotalCalories,
  SUM(HourlySteps.Total_Steps) AS TotalSteps,
  EXTRACT(DATE FROM HourlyIntensities.ActivityHour) AS Date,
  EXTRACT(YEAR FROM HourlyIntensities.ActivityHour) AS Year,
  EXTRACT(MONTH FROM HourlyIntensities.ActivityHour) AS Month,
  EXTRACT(DAY FROM HourlyIntensities.ActivityHour) AS Day,
  FORMAT_DATE('%A', DATE(HourlyIntensities.ActivityHour)) AS DayName,
  CAST(EXTRACT(TIME FROM HourlyIntensities.ActivityHour) AS TIME) AS Time,
  EXTRACT(HOUR FROM HourlyIntensities.ActivityHour) AS Hour
FROM `gda-course-4-332812.Capstone.HourlyIntensities` AS HourlyIntensities
LEFT JOIN 
  (SELECT Id, ActivityHour, SUM(Calories) AS Total_Calories,
  EXTRACT(YEAR FROM ActivityHour) AS Year,
  EXTRACT(MONTH FROM ActivityHour) AS Month,
  EXTRACT(DAY FROM ActivityHour) AS Day,
  EXTRACT(DAYOFWEEK FROM ActivityHour) AS DayName,
  EXTRACT(Hour FROM ActivityHour) AS Hour,
  FROM `gda-course-4-332812.Capstone.HourlyCalories` AS Hourly_Calories
  GROUP BY Id, ActivityHour) AS HourlyCalories
ON HourlyIntensities.Id = HourlyCalories.Id
AND HourlyIntensities.ActivityHour = HourlyCalories.ActivityHour
LEFT JOIN 
  (SELECT Id, ActivityHour, SUM(StepTotal) AS Total_Steps,
  EXTRACT(YEAR FROM ActivityHour) AS Year,
  EXTRACT(MONTH FROM ActivityHour) AS Month,
  EXTRACT(DAY FROM ActivityHour) AS Day,
  EXTRACT(DAY FROM ActivityHour) AS DayName,
  EXTRACT(Hour FROM ActivityHour) AS Hour,
  COUNT(*) AS Duplicates
  FROM `gda-course-4-332812.Capstone.HourlySteps` AS Hourly_Steps
  GROUP BY Id, ActivityHour
  ORDER BY Id, ActivityHour) AS HourlySteps
ON HourlyIntensities.Id = HourlySteps.Id
AND HourlyIntensities.ActivityHour = HourlySteps.ActivityHour
GROUP BY HourlyIntensities.Id, HourlyIntensities.ActivityHour
ORDER BY HourlyIntensities.Id, HourlyIntensities.ActivityHour

--METs per minute, extracted year, month, day, day name, hour 
-- No duplicates found 
-- Aggregated into METs per day
WITH Minute_METs AS (SELECT Id, SUM(METs) AS METs,
EXTRACT(DATE FROM ActivityMinute) AS Date,
  EXTRACT(YEAR FROM ActivityMinute) AS Year,
  EXTRACT(MONTH FROM ActivityMinute) AS Month,
  EXTRACT(DAY FROM ActivityMinute) AS Day,
  FORMAT_DATE('%A', DATE(ActivityMinute)) AS DayName,
  CAST(EXTRACT(TIME FROM ActivityMinute) AS TIME) AS Time,
  EXTRACT(HOUR FROM ActivityMinute) AS Hour,
  COUNT(*) AS Duplicates
FROM `gda-course-4-332812.Capstone.MinuteMETs` AS MinuteMETS
GROUP BY Id, ActivityMinute
ORDER BY Id, ActivityMinute)

SELECT Id, Date, SUM(METs) AS Total_METs, ROUND(AVG(METs), 2) AS Avg_METs,
  EXTRACT(YEAR FROM Date) AS Year,
  EXTRACT(MONTH FROM Date) AS Month,
  EXTRACT(DAY FROM Date) AS Day,
  FORMAT_DATE('%A', DATE(Date)) AS DayName
FROM Minute_METs
GROUP BY Id, Date
```

### Brief Summary:
- DailyActivity and SleepDay tables were combined into one table.
- Hourly tables (*HourlyIntensities, HourlyCalories, and HourlySteps*) were combined into one table.
- HourlyMETSs table was aggregated into daily METs.
