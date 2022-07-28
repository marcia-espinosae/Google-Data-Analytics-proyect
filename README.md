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

### ASK (1. A clear statement of the business task)

Moreno has assigned me this question:

    * HOW DO ANNUAL MEMBERS AND CASUAL RIDERS USE CYCLISTIC BIKES DIFFERENTLY?
   


5. Supporting visualizations and key findings
6. Your top three recommendations based on your analysis
    
### PREPARE (2. A description of all data sources used)

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
### PROCESS (3. Documentation of any cleaning or manipulation of data)

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
(1) The data can only be aggregated at the ride-level, which is too granular. We will want to add some additional columns of data — such as day, month,
year — that provide additional opportunities to aggregate the data.
(2) We will want to add a calculated field for length of ride, create “tripduration” column. We will add “ride_length” to the entire data frame.

      Problem Solving:
   (1) 
   ```
   selected_data$date <- as.Date(selected_data$started_at) #Format is yyyy-mm-dd
   selected_data$month <- format(as.Date(selected_data$date), "%m")
   selected_data$day <- format(as.Date(selected_data$date), "%d")
   selected_data$year <- format(as.Date(selected_data$date), "%Y")
   selected_data$day_of_week <- format(as.Date(selected_data$date), "%A") 
   ```
   (2)
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


Remove "bad" data
##### The dataframe includes a few entries where ride_length was negative. (5757551 obs. of  15 variables)
```
nrow(selected_data[(selected_data$ride_length<0),])
       [1] 140
```
##### We will create a new version of the dataframe (v2) whitout ride_length negative.
```
selected_data_v2 <- selected_data[!(selected_data$ride_length<0),]
dim(selected_data_v2)
       [1] 5757411      15
```
### ANALYZE AND VISUALIZATIONS.

# Number of members vs. casual riders
```
table(selected_data_v2$member_casual)
```
| Members  | Casual |
| ----- | ------ |
| 3221113 | 2536298 | 
   
# Descriptive analysis on ride_length (all figures in seconds)
```
mean(selected_data_v2$ride_length) 
median(selected_data_v2$ride_length) 
max(selected_data_v2$ride_length)
min(selected_data_v2$ride_length) 
```

| Mean  | Median | Max  | Min  |
| ----- | ------ | ---- | ---- |
| 1268 | 691 | 3356649 | 0 |

# Compare members and casual users
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
 
 
 # See the average ride time by each day for members vs casual users
 ```
aggregate(selected_data_v2$ride_length ~ selected_data_v2$member_casual + selected_data_v2$day_of_week, FUN = mean)
      selected_data_v2$member_casual selected_data_v2$day_of_week selected_data_v2$ride_length
1                          casual                       Sunday               2218.1479 secs
2                          member                       Sunday                903.7009 secs
3                          casual                       Monday               1863.9642 secs
4                          member                       Monday                762.7473 secs
5                          casual                      Tuesday               1587.8459 secs
6                          member                      Tuesday                735.5809 secs
7                          casual                    Wednesday               1625.5655 secs
8                          member                    Wednesday                744.9586 secs
9                          casual                     Thursday               1673.2608 secs
10                         member                     Thursday                746.2896 secs
11                         casual                       Friday               1752.5362 secs
12                         member                       Friday                772.6787 secs
13                         casual                     Saturday               2051.5933 secs
14                         member                     Saturday                886.8511 secs
```

# analyze ridership data by type and weekday
```
selected_data_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  #creates weekday field using wday()
  group_by(member_casual, weekday) %>%  #groups by usertype and weekday
  summarise(number_of_rides = n()				#calculates the number of rides and average duration 
  ,average_duration = mean(ride_length)) %>% # calculates the average duration
  arrange(member_casual, weekday)	
```


### The Share phase 

#### Conclusions/Summary of insights 

Members and casual riders differ in how long they use the bikes, how often they use the bikes, and on which days of the week does every group peak:

* Casual rides peak during weekends (plot3). There is a high probability they are tourists visiting and sightseeing the city, or that they are ordinary Chicago residents who are riding bike in their leisure time during the weekend. The longer average ride time for casual rider (plot2), also peaking at the weekend,  provides evidence for this point.

* Ride length for members are relatively shorter compared to casual riders. This could clarified as such, that most members use the bikes to commute on workdays. This clarification would also explain the short riding durations of members. They ride from point A to B, namely roughly always the same ride lengths and the same distance

* Ridership start to pick up (plot8) from February (from Spring through Summer)and start to decrease in August (from Fall through winter). This correlation is due to the seasonal changes. As the weather start to get warmer and more pleasant in February (start of Spring), more people starts to cycle, and inversely when the weather  becomes less warm cold around September (start of Fall).

* More than 50% of the riders are annual members (plot5), suggesting that the company have already achieved a certain level of loyalty among its bike users. This indicates a positive message, namely that the company is going to be able to convince many casual riders to convert to members, and to keep the new members satisfied. 


#### Recommendations

Give discounts for longer rides when you have a membership
Longer rides can get some type of rewards program when they become members

* The marketing campaign should be launched between February to August, as the number of trips made by casual riders peaks at this time of the year.

* As casual rider usage reach its highest point on the weekend, the marketing campaign can include weekend-only membership at a sensible price. This could attract casual riders to convert to members.

* The campaign could include ride-length-based tariff plan (maybe only on weekends): Bike more, pay less ! 
This provides more incentive for the member rides to cycle longer distances. 

* Alternatively, longer rides can be rewarded with benefits such as discount vouchers.
  
