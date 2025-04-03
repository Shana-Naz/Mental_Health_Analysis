# Mental Health Analysis Among Teenagers

## SQL Queries
### Create Database
CREATE DATABASE m_health;

### Create Table
CREATE TABLE mental_health
(
   User_id INT PRIMARY KEY,
   Age INT,
   Gender CHAR,
   Social_media_hours FLOAT,
   Exercise_hours FLOAT,
   Sleep_hours FLOAT,
   Screen_time_hours FLOAT,
   Survey_stress_score INT,
   Wearable_stress_score FLOAT,
   Support_system VARCHAR(20),
   Academic_performance VARCHAR(50)
   );

   ### Display all the contents of table
   SELECT * FROM mental_health;

   ### How does social media usage affect stress levels?
   SELECT 
   
   CASE  
   
       WHEN Social_media_hours < 2 THEN 'Low (0-2 hrs)'
       
       WHEN Social_media_hours BETWEEN 2 AND 4 THEN 'Moderate (2-4 hrs)'
       
       WHEN Social_media_hours > 4 THEN 'High (4-6 hrs)'
       
       ELSE 'Very high'
       
   END AS social_media_usage_grp,
   
   ROUND(AVG(Survey_stress_score),2) AS avg_survey_stress,
   
   ROUND(AVG(Wearable_stress_score),2) AS avg_wearable_stress,
   
   COUNT(*) AS User_count
   
FROM mental_health

GROUP BY social_media_usage_grp

ORDER BY avg_survey_stress DESC;

#### Correlation between social media usage and survey stress score
SELECT 

    (COUNT(*) * SUM(Social_Media_Hours * Survey_Stress_Score) - SUM(Social_Media_Hours) * SUM(Survey_Stress_Score)) / 
    (SQRT((COUNT(*) * SUM(Social_Media_Hours * Social_Media_Hours) - POWER(SUM(Social_Media_Hours), 2)) * 
          (COUNT(*) * SUM(Survey_Stress_Score * Survey_Stress_Score) - POWER(SUM(Survey_Stress_Score), 2)))) 
          
    AS Correlation_survey_stress
    
FROM mental_health;

#### Correlation between social media usage and wearable stress score
SELECT 

    (COUNT(*) * SUM(Social_Media_Hours * Wearable_stress_score) - SUM(Social_Media_Hours) * SUM(Wearable_stress_score)) / 
    (SQRT((COUNT(*) * SUM(Social_Media_Hours * Social_Media_Hours) - POWER(SUM(Social_Media_Hours), 2)) * 
          (COUNT(*) * SUM(Wearable_stress_score * Wearable_stress_score) - POWER(SUM(Wearable_stress_score), 2)))) 
          
    AS Correlation_wearable_stress
    
FROM mental_health;

### Does sleep duration impact academic performance?
SELECT

   Academic_performance,
   
   ROUND(AVG(Sleep_hours),2) AS avg_sleep_hrs,
   
   COUNT(*) AS user_count
   
FROM
   mental_health
   
GROUP BY

   Academic_performance
   
ORDER BY

   avg_sleep_hrs DESC;

### Is there a difference in stress levels between genders?
SELECT

     Gender,
     
     ROUND(AVG(Survey_stress_score),2) AS avg_survey_stress,
     
     ROUND(AVG(Wearable_stress_score),2) AS avg_wearable_stress
FROM 

     mental_health
     
GROUP BY

     Gender;

### How does exercise influence stress levels?
SELECT 

    CASE
    
        WHEN Exercise_hours = 0 THEN 'No Exercise'
        
        WHEN Exercise_hours BETWEEN 0.1 AND 1.9 THEN 'Low Exercise'
        
        WHEN Exercise_hours BETWEEN 2 AND 3.9 THEN 'Moderate Exercise'
        
        ELSE 'High Exercise'
        
    END AS exercise_pattern,
    
    ROUND(AVG(Survey_stress_score), 2) AS avg_survey_stress,
    
    ROUND(AVG(Wearable_stress_score), 2) AS avg_wearable_stress,
    
    COUNT(*) AS user_count
    
FROM
    mental_health
    
GROUP BY exercise_pattern

ORDER BY avg_survey_stress DESC;

### Does having a strong support system correlate with lower stress?
SELECT 

     Support_system,
     
     ROUND(AVG(Survey_stress_score),2) AS avg_survey_stress,
     
     ROUND(AVG(Wearable_stress_score),2) AS avg_wearable_stress,
     
     COUNT(*) AS user_count
     
FROM

     mental_health
     
GROUP BY 

     Support_system
     
ORDER BY

     avg_survey_stress;

### Does increased screen time contribute to higher stress levels?
SELECT 

    (COUNT(*) * SUM(Screen_Time_Hours * Survey_Stress_Score) - SUM(Screen_Time_Hours) * SUM(Survey_Stress_Score)) / 
    (SQRT((COUNT(*) * SUM(Screen_Time_Hours * Screen_Time_Hours) - POWER(SUM(Screen_Time_Hours), 2)) * 
          (COUNT(*) * SUM(Survey_Stress_Score * Survey_Stress_Score) - POWER(SUM(Survey_Stress_Score), 2)))) 
          
    AS ScreenTime_Stress_Correlation,
    
    (COUNT(*) * SUM(Screen_Time_Hours * Wearable_stress_score) - SUM(Screen_Time_Hours) * SUM(Wearable_stress_score)) / 
    (SQRT((COUNT(*) * SUM(Screen_Time_Hours * Screen_Time_Hours) - POWER(SUM(Screen_Time_Hours), 2)) * 
          (COUNT(*) * SUM(Wearable_stress_score * Wearable_stress_score) - POWER(SUM(Wearable_stress_score), 2)))) 
          
    AS ScreenTime_wearable_Stress_Correlation
    
FROM 

    mental_health;

### Do younger individuals experience higher stress levels compared to older individuals in the dataset?
SELECT

    CASE
    
        WHEN Age < 14 THEN 'Young Individuals'
        
        WHEN Age BETWEEN 14 AND 16 THEN 'Middle Individuals'
        
        WHEN Age > 16 THEN 'Older Individuals'
        
        ELSE 'Grownups'
        
	END AS Age_category,
 
    ROUND(AVG(survey_stress_score),2) AS avg_survey_stress,
    
    ROUND(AVG(Wearable_stress_score),2) AS avg_wearable_stress,
    
    COUNT(*) AS user_count
    
FROM 

    mental_health
    
GROUP BY

    Age_category
    
ORDER BY

    avg_survey_stress;



   
