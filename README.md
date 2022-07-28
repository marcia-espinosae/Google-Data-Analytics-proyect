# Google-Data-Analytics-proyect
**Cyclistic Case Study**

**Author: Marcia Espinosa**

**Download Data: "27/05/2022"**

### Introduction

To complete my certification in google data analysis, I chose the first case study, "cyclists". In order to carry out the project, I installed R studio where it does all the work based on: Ask, prepare, process, analyze, share, and act.

### Proyect_Scenario

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

### ASK 

Moreno has assigned me this question:

    * HOW DO ANNUAL MEMBERS AND CASUAL RIDERS USE CYCLISTIC BIKES DIFFERENTLY?
    
### PREPARE 

Data sources: 12 months from 01/05/2021 to 01/04/2022 
Download from: https://divvy-tripdata.s3.amazonaws.com/index.html

Data Credibility (ROCCC): 
For the purposes of this case study, the datasets are appropriate and will enable you to answer the business questions. The data has been made available 
by Motivate International Inc. under this license https://ride.divvybikes.com/data-license-agreement. This is public data that you can use to explore how
different customer types are using Cyclistic bikes. But note that data-privacy issues prohibit you from using riders’ personally identifiable information.
This means that you won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if
they have purchased multiple single passes.

File Type:  .csv  format for every month.

I will be using Rstudio to perform this analysis. 

**** Packages and Library
```
install.packages("tidyverse")
install.packages("skimr")

library(tidyverse)
library(skimr)
library(readr)
library(hms) 
library(lubridate)
library(ggplot2)
library(dplyr)
```

**** Importing Data
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
**** Compare column names each of the files
```
colnames(tripdata_21may) 
colnames(tripdata_21jun)
colnames(tripdata_21jul)
colnames(tripdata_21ago)
colnames(tripdata_21sep)
colnames(tripdata_21oct)
colnames(tripdata_21nov)
colnames(tripdata_21dec)
colnames(tripdata_22jan)
colnames(tripdata_22feb)
colnames(tripdata_22mar)
colnames(tripdata_22abr)
```
     * Column names for each file (the same 13 variables)
       [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"          
       [5] "start_station_name" "start_station_id"   "end_station_name"   "end_station_id"    
       [9] "start_lat"          "start_lng"          "end_lat"            "end_lng"           
       [13] "member_casual"     

**** Join Data. Name: Data (5757551 obs. of 13 variables)
```
data <- rbind(tripdata_21may, 
              tripdata_21jun, 
              tripdata_21jul, 
              tripdata_21ago, 
              tripdata_21sep, 
              tripdata_21oct, 
              tripdata_21nov,
              tripdata_21dec,
              tripdata_22jan,
              tripdata_22feb,
              tripdata_22mar,
              tripdata_22abr)        
```
### PROCESS 

**Inspect the new table that has been created (Name:data)
   
   * Duplicate 
```
sum(duplicated(data$ride_id))
[1] 0
```
   * List column that I want to drop, since the data is incomplete. New Name: selected_data
```
drop <- c('start_station_name', 'start_station_id', 'end_station_name', 'end_station_id')
selected_data = data[,!(names(data) %in% drop)] 
```
   * Review new table (selected_data)
```
colnames(selected_data)
   [1] "ride_id"       "rideable_type" "started_at"    "ended_at"      "start_lat"     "start_lng"    
   [7] "end_lat"       "end_lng"       "member_casual"
   
dim(selected_data)
   [1] 5757551       9

nrow(selected_data)
   [1] 5757551

str(selected_data)
   Classes ‘spec_tbl_df’, ‘tbl_df’, ‘tbl’ and 'data.frame':	5757551 obs. of  9 variables:
 $ ride_id      : chr  "C809ED75D6160B2A" "DD59FDCE0ACACAF3" "0AB83CB88C43EFC2" "7881AC6D39110C60" ...
 $ rideable_type: chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
 $ started_at   : POSIXct, format: "2021-05-30 11:58:15" "2021-05-30 11:29:14" "2021-05-30 14:24:01" ...
 $ ended_at     : POSIXct, format: "2021-05-30 12:10:39" "2021-05-30 12:14:09" "2021-05-30 14:25:13" ...
 $ start_lat    : num  41.9 41.9 41.9 41.9 41.9 ...
 $ start_lng    : num  -87.6 -87.6 -87.7 -87.7 -87.7 ...
 $ end_lat      : num  41.9 41.8 41.9 41.9 41.9 ...
 $ end_lng      : num  -87.6 -87.6 -87.7 -87.7 -87.7 ...
 $ member_casual: chr  "casual" "casual" "casual" "casual" ...
```
  *  Problems we will need to fix:
  
1. The data can only be aggregated at the ride-level, which is too granular. We will want to add some additional columns of data — such as day, month,
year — that provide additional opportunities to aggregate the data.

2. We will want to add a calculated field for length of ride, create “tripduration” column. We will add “ride_length” to the entire data frame.

        Problem Solving:
      
     1.
   ```
   selected_data$date <- as.Date(selected_data$started_at) #Format is yyyy-mm-dd
   selected_data$month <- format(as.Date(selected_data$date), "%m")
   selected_data$day <- format(as.Date(selected_data$date), "%d")
   selected_data$year <- format(as.Date(selected_data$date), "%Y")
   selected_data$day_of_week <- format(as.Date(selected_data$date), "%A") 
   ```
   
     2.
   ```
   selected_data$ride_length <- difftime(selected_data$ended_at, selected_data$started_at)
   ```
* Review 
```
colnames(selected_data)
     [1] "ride_id"       "rideable_type" "started_at"    "ended_at"      "start_lat"     "start_lng"    
     [7] "end_lat"       "end_lng"       "member_casual" "date"          "month"         "day"          
     [13] "year"          "day_of_week"   "ride_length"  
str(selected_data)
     Classes ‘spec_tbl_df’, ‘tbl_df’, ‘tbl’ and 'data.frame':	5757551 obs. of  15 variables:
     $ ride_id      : chr  "C809ED75D6160B2A" "DD59FDCE0ACACAF3" "0AB83CB88C43EFC2" "7881AC6D39110C60" ...
     $ rideable_type: chr  "electric_bike" "electric_bike" "electric_bike" "electric_bike" ...
     $ started_at   : POSIXct, format: "2021-05-30 11:58:15" "2021-05-30 11:29:14" "2021-05-30 14:24:01" ...
     $ ended_at     : POSIXct, format: "2021-05-30 12:10:39" "2021-05-30 12:14:09" "2021-05-30 14:25:13" ...
     $ start_lat    : num  41.9 41.9 41.9 41.9 41.9 ...
     $ start_lng    : num  -87.6 -87.6 -87.7 -87.7 -87.7 ...
     $ end_lat      : num  41.9 41.8 41.9 41.9 41.9 ...
     $ end_lng      : num  -87.6 -87.6 -87.7 -87.7 -87.7 ...
     $ member_casual: chr  "casual" "casual" "casual" "casual" ...
     $ date         : Date, format: "2021-05-30" "2021-05-30" "2021-05-30" ...
     $ month        : chr  "05" "05" "05" "05" ...
     $ day          : chr  "30" "30" "30" "30" ...
     $ year         : chr  "2021" "2021" "2021" "2021" ...
     $ day_of_week  : chr  "Sunday" "Sunday" "Sunday" "Sunday" ...
     $ ride_length  : 'difftime' num  744 2695 72 913 ...
      ..- attr(*, "units")= chr "secs"
```


#### Remove "bad" data
* The dataframe includes a few entries where ride_length was negative. (5757551 obs. of  15 variables)
```
nrow(selected_data[(selected_data$ride_length<0),])
       [1] 140
```
* We will create a new version of the dataframe (v2) whitout ride_length negative.
```
selected_data_v2 <- selected_data[!(selected_data$ride_length<0),]
dim(selected_data_v2)
       [1] 5757411      15
```
### ANALYZE AND VISUALIZATIONS.

* Number of members vs casual riders
```
table(selected_data_v2$member_casual)
ggplot(selected_data_v2, aes(member_casual, fill = member_casual)) + geom_bar() +
  labs(title = "Type of Riders", x = "Customer Type")
```
| Members  | Casual |
| ----- | ------ |
| 3221113 | 2536298 | 

![Rplot01](https://user-images.githubusercontent.com/109522662/181612261-b697e995-6fa0-4409-8590-cd55de8eafbd.png)

   
* Descriptive analysis on ride_length (all figures in seconds)
```
mean(selected_data_v2$ride_length) 
median(selected_data_v2$ride_length) 
max(selected_data_v2$ride_length)
min(selected_data_v2$ride_length) 
```

| Mean  | Median | Max  | Min  |
| ----- | ------ | ---- | ---- |
| 1268 | 691 | 3356649 | 0 |

* Compare members and casual users
```
aggregate(selected_data_v2$ride_length ~ selected_data_v2$member_casual, FUN = mean)
aggregate(selected_data_v2$ride_length ~ selected_data_v2$member_casual, FUN = median)
aggregate(selected_data_v2$ride_length ~ selected_data_v2$member_casual, FUN = max)
aggregate(selected_data_v2$ride_length ~ selected_data_v2$member_casual, FUN = min)
```     
Members:
           
| Mean  | Median | Max     | Min  |             
| ----- | ------ | ----    | ---- |             
| 788   | 551   | 93594 | 0    |  |     

Casual:
 
 |  Mean  | Median | Max  | Min  |
 | ----- | ------ | ---- | ----  |
|   1877 | 934| 3356649 | 0     |
 
 
* Average ride time by each day for members vs casual users + visualization.
 ```
aggregate(selected_data_v2$ride_length ~ selected_data_v2$member_casual + selected_data_v2$day_of_week, FUN = mean)

selected_data_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  #creates weekday field using wday()
  group_by(member_casual, weekday) %>%  #groups by usertype and weekday
  summarise(number_of_rides = n()				#calculates the number of rides and average duration 
  ,average_duration = mean(ride_length)) %>% # calculates the average duration
  arrange(member_casual, weekday)	
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge") + scale_fill_manual(values = c("#66b2ff", "#6666ff"))
```
![Rplot2](https://user-images.githubusercontent.com/109522662/181620151-ed2ee838-be4b-478b-b106-9826c44213be.png)

* Visualization for average duration vs weekdays
```
selected_data_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n()
  ,average_duration = mean(as.numeric(ride_length))) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge")
```
![Rplot3](https://user-images.githubusercontent.com/109522662/181621138-bb52b71a-93f5-445f-9981-a1cef9b3108f.png) 

* Visualization for Average duration vs months
```
selected_data_v2 %>% 
  mutate(month=month(started_at, label = TRUE)) %>%
  group_by(member_casual, month) %>% 
  summarise(number_of_rides = n()
  ,average_duration = mean(as.numeric(ride_length))) %>% 
  arrange(member_casual, month)  %>% 
  ggplot(aes(x = month, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge") + scale_fill_manual(values = c("#66b2ff", "#6666ff"))
  ```
  ![Rplot4](https://user-images.githubusercontent.com/109522662/181633490-08954fe1-5fc5-411a-a00b-31673cef8607.png)


### Conclusions/Findings
* The company has more MEMBER subscribers.

* Number of riders vs weekdays = members show preferences on weekdays, with a pick on Wednesday. The casual ones show more preferences on weekends with a pick on Saturdays.

* Average_duration vs weekdays = members show a stable average throughout the week, showing a slight rise on weekends, but staying below 1000 secs. Casual riders show a much higher average, over 1500 secs, showing an increase on weekends over 2000 secs.

* Average_duration vs months = The members show a stable duration, which has a slight increase in the months of May, Jun, Jul, Aug, Sep, but remains below 1000 secs. Casual riders show more fluctuation throughout the year, showing spikes above 2000 secs in the months of May and June, and drops below 1500 secs in the months of Nov. and Dec.

### Recommendations

Considering that this analysis contemplates only one question, which is how the annual members behave in relation to the casual ones, it is not possible to give an accurate conclusion to make a marketing proposal.

My recommendation is that a more complete analysis is required that includes more personal information of the riders, analyzes other variables such as location, weather.

Perhaps it would be good to do a small survey to the same users, so that they themselves contribute with their requests.

It would be good to carry out an informative campaign based on prices and discounts.

If we assume that both members and casual users use the service through a digital medium, such as a web application, then an individualized campaign could be carried out according to their behavior and based on these improve the benefits of membership, and see the impact of each strategy.



