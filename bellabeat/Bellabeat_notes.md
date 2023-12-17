# Bellabeat Case Study

/*
Bellabeat Case Study<br>
<br>
Data cleaning: SleepDay column in sleepDay_merged is divided into SleepDay and SleepTime columns by using the following formulas on Google Sheet<br>
  =LEFT(B2,FIND(" ",B2)-1)<br>
  =RIGHT(B2,LEN(B2)-FIND(" ",B2))<br>
*/<br>
<br>
-- Count the number of IDs in each table<br>
<br>
SELECT<br>
COUNT(DISTINCT Id)<br>
FROM<br>
`bellabeat-405622.fitbit.dailyActivity_merged`<br>
<br>
<br>
-- Check if dailySteps_merged is needed<br>
<br>
SELECT<br>
dailyActivity.Id,<br>
ActivityDate<br>
FROM<br>
`bellabeat-405622.bellabeat.dailyActivity_merged` AS dailyActivity<br>
INNER JOIN<br>
`bellabeat-405622.bellabeat.dailySteps_merged` AS dailySteps<br>
ON<br>
dailyActivity.Id = dailySteps.Id<br>
AND<br>
dailyActivity.ActivityDate = dailySteps.ActivityDay<br>
WHERE<br>
TotalSteps <> StepTotal<br>
<br>

-- A relationship between activitiy level and calories<br>
<br>
SELECT<br>
  Id,<br>
  ActivityDate,<br>
  Calories,<br>
  TotalSteps,<br>
  TotalDistance,<br>
  (VeryActiveDistance+ModeratelyActiveDistance) AS TotalActiveDistance,<br>
  (VeryActiveMinutes+FairlyActiveMinutes) AS TotalActiveMinutes<br>
FROM<br>
  `bellabeat-405622.fitbit.dailyActivity_merged`<br>
ORDER BY<br>
  Calories DESC<br>
<br>
![1-activity_level_and_calories.png](attachment:1-activity_level_and_calories.png)
<br>

-- Combine columns from weightLogInfo_merged and sleepDay_merged tables into dailyActivity_merged table<br>
<br>
SELECT<br>
  dailyActivity.Id,<br>
  Calories,<br>
  WeightKg,<br>
  BMI,<br>
  ActivityDate,<br>
  TotalSteps,<br>
  TotalDistance,<br>
  VeryActiveDistance,<br>
  ModeratelyActiveDistance,<br>
  LightActiveDistance,<br>
  SedentaryActiveDistance,<br>
  VeryActiveMinutes,<br>
  FairlyActiveMinutes,<br>
  LightlyActiveMinutes,<br>
  SedentaryMinutes,<br>
  TotalMinutesAsleep,<br>
  TotalTimeInBed<br>
FROM<br>
  `bellabeat-405622.bellabeat.dailyActivity_merged` As dailyActivity<br>
LEFT JOIN<br>
  `bellabeat-405622.bellabeat.weightLogInfo_merged` AS weightLog<br>
  ON<br>
    dailyActivity.Id = weightLog.Id<br>
    AND<br>
    dailyActivity.ActivityDate = weightLog.Date<br>
LEFT JOIN<br>
   `bellabeat-405622.bellabeat.sleepDay_merged` AS sleepDay<br>
ON<br>
    dailyActivity.Id = sleepDay.Id<br>
    AND<br>
    dailyActivity.ActivityDate = sleepDay.SleepDay<br>
<br>

-- A relationship between activitiy level and sleep<br>
![7-activity_level_and_sleep.png](attachment:7-activity_level_and_sleep.png)<br>
<br>

-- A relationship between the hour of the day and activity intensity<br>
![8-hour_and_intensity.png](attachment:8-hour_and_intensity.png)<br>
<br>

-- The average minutes of each activity level<br>
![10-active_minutes_pie_chart.png](attachment:10-active_minutes_pie_chart.png)<br>
<br>

##Findings
- The longer the users' active distance is, the more they burn calories
- The longer the users' have sedentary minutes, the shorter sleep time they have
- The activity intensity increases towards the middle of the day and peaks at 6 pm, and then decreases towards the midnight
- More than 80% of the time is recorded as sedentary minutes


##Recommendations
- Notify users how many sedentary minutes they have on the day so far to encourage them to be active
- Set a goal of active minutes and calories burned a day and let users know when they achieve it
- Based on the active minutes or calories burned, give users points that can be redeemed for the subscriptions or products
