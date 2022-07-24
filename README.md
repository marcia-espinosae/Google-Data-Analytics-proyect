# Google-Data-Analytics-proyect
**Cyclistic Case Study**

**Author: Marcia Espinosa**

**Download Data: "27/05/2022"**

# Introduction

To complete my certification in google data analysis, I chose the first case study, "cyclists". In order to carry out the project, I installed R studio where it does all the work based on: Ask, prepare, process, analyze, share, and act.

# Proyect_Scenario

The project is for a company in Chicago created in 2016, which successfully performs "bike_share". 
Cyclistic has 3 flexible pricing plans:  
  * Single-ride passes, 
  * Full-day passes, and 
  * Annual memberships. 

Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

"Cyclistic's finance analysts have concluded that annual members are much more profitable than casual riders". The director, Lily Moreno, believes that 
in order for the company to grow successfully, the number of annual members must be increased.

Proyect_Goal: Design marketing strategies aimed al converting casual riders into annual members.

Question for marketing team:
  1. How do annual members and casual riders use Cyclistic bikes differently?
  2. Why would casual riders buy Cyclistic annual memberships?
  3. How can Cyclistic use digital media to influence casual riders to become members?

## You will produce a report with the following deliverables:

# ASK (1. A clear statement of the business task)

Moreno has assigned me this question:

    * HOW DO ANNUAL MEMBERS AND CASUAL RIDERS USE CYCLISTIC BIKES DIFFERENTLY?
   
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Your top three recommendations based on your analysis
    
# Prepare (2. A description of all data sources used)

Data sources: 12 months from 01/05/2021 to 01/04/2022  download from: https://divvy-tripdata.s3.amazonaws.com/index.html

DATA CREDIBILITY:
For the purposes of this case study, the datasets are appropriate and will enable you to answer the business questions. The data has been made available 
by Motivate International Inc. under this license https://ride.divvybikes.com/data-license-agreement. This is public data that you can use to explore how
different customer types are using Cyclistic bikes. But note that data-privacy issues prohibit you from using riders’ personally identifiable information.
This means that you won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if
they have purchased multiple single passes.

File Type:  .csv  format for every month.

```
tripdata_21may <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21may.csv")
tripdata_21jun <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21jun.csv")
tripdata_21jul <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21jul.csv")
tripdata_21ago <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21ago.csv")
tripdata_21sep <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21sep.csv")
tripdata_21oct <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21oct.csv")
tripdata_21nov <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21nov.csv")
tripdata_21dec <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_21dec.csv")
tripdata_22jan <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_22jan.csv")
tripdata_22feb <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_22feb.csv")
tripdata_22mar <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_22mar.csv")
tripdata_22abr <- read_csv("Desktop/case_study/cyclistic_trip_data/tripdata_22abr.csv")
```


I used R for data verification and cleaning: Reasons:
The 12 data sets combined will contain more than 5 million rows of data. Excel worksheet limitation is 1,048,576 rows. Moreover, some csv files could not uploaded to BigQuery for file size problems. Thus, R is used to perform all tasks from organizing, cleaning analyzing and visualizing data.

 

PRO

  ***






  
