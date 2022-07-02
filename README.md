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

SELECT *
FROM `expanded-bebop-352104.fitbit.MinuteMETs` AS MinuteMETS
