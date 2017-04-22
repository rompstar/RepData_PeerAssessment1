---
title: "Peer-graded Assignment: Course Project 1"
author: "Raymond Miecznik"
date: "4/10/2017"
output: html_document
---

## dependencies and Loading and preprocessing the data

# Show any code that is needed to

Load the data (i.e. read.csv())
Process/transform the data (if necessary) into a format suitable for your analysis


```r
library(ggplot2)
library(sqldf)
library(dplyr)

# Loading and preprocessing the data
# 
# Show any code that is needed to

system('git clone https://github.com/rompstar/RepData_PeerAssessment1')

setwd("RepData_PeerAssessment1")
getwd()
```

```
## [1] "/Users/RAY/R/RepData_PeerAssessment1/RepData_PeerAssessment1"
```

```r
# unzip the file
unzip("activity.zip")

# load the data into a data.frame
activity <- read.csv(file="activity.csv", header=TRUE, sep=",")

# format the date as.Date type
activity$date <- as.Date(activity$date)

summary(activity)
```

```
##      steps             date               interval     
##  Min.   :  0.00   Min.   :2012-10-01   Min.   :   0.0  
##  1st Qu.:  0.00   1st Qu.:2012-10-16   1st Qu.: 588.8  
##  Median :  0.00   Median :2012-10-31   Median :1177.5  
##  Mean   : 37.38   Mean   :2012-10-31   Mean   :1177.5  
##  3rd Qu.: 12.00   3rd Qu.:2012-11-15   3rd Qu.:1766.2  
##  Max.   :806.00   Max.   :2012-11-30   Max.   :2355.0  
##  NA's   :2304
```


## What is mean total number of steps taken per day?

# For this part of the assignment, you can ignore the missing values in the dataset.

Calculate the total number of steps taken per day
If you do not understand the difference between a histogram and a barplot, research the difference between them. 
Make a histogram of the total number of steps taken each day
Calculate and report the mean and median of the total number of steps taken per day


```r
daily <- tapply(activity$steps, activity$date, sum, na.rm=TRUE)

head(daily)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##          0        126      11352      12116      13294      15420
```

```r
# plot the daily sum of steps
qplot(daily,
      geom="histogram",
      binwidth = 600,
      main = "Histogram for Daily Sum of Steps",
      fill=I("blue"),
      col=I("red"),
      alpha=I(.2))
```

![plot of chunk daily](figure/daily-1.png)

```r
# report the mean and median total number of steps taken per day
daily_mean <- mean(daily)
daily_median <- median(daily)
```

## What is the average daily activity pattern?

Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

![plot of chunk 5min_interval](figure/5min_interval-1.png)![plot of chunk 5min_interval](figure/5min_interval-2.png)

```
##   interval     mean
## 1      835 206.1698
```

## Imputing missing values

Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)
Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
    
Create a new dataset that is equal to the original dataset but with the missing data filled in.
Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```
##   count(*)
## 1     2304
```

```
##   count(*)
## 1        0
```

```
##   count(*)
## 1        0
```

![plot of chunk impute](figure/impute-1.png)

```
##       date                steps            which          
##  Min.   :2012-10-02   Min.   : 0.1424   Length:53         
##  1st Qu.:2012-10-16   1st Qu.:30.6979   Class :character  
##  Median :2012-10-29   Median :37.3785   Mode  :character  
##  Mean   :2012-10-30   Mean   :37.3826                     
##  3rd Qu.:2012-11-16   3rd Qu.:46.1597                     
##  Max.   :2012-11-29   Max.   :73.5903
```

```
##       date                steps            which          
##  Min.   :2012-10-01   Min.   : 0.1424   Length:61         
##  1st Qu.:2012-10-16   1st Qu.:34.0938   Class :character  
##  Median :2012-10-31   Median :37.3826   Mode  :character  
##  Mean   :2012-10-31   Mean   :37.3826                     
##  3rd Qu.:2012-11-15   3rd Qu.:44.4826                     
##  Max.   :2012-11-30   Max.   :73.5903
```

```
##   sum(steps)
## 1   1981.278
```

```
##   sum(steps)
## 1   2280.339
```


## Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```
##       steps       date interval day_type
## 1 1.7169811 2012-10-01        0  weekday
## 2 0.3396226 2012-10-01        5  weekday
## 3 0.1320755 2012-10-01       10  weekday
## 4 0.1509434 2012-10-01       15  weekday
## 5 0.0754717 2012-10-01       20  weekday
## 6 2.0943396 2012-10-01       25  weekday
```

```
##   day_type
## 1  weekday
## 2  weekend
```

```
##   interval day_type      steps
## 1        0  weekday 1.94375222
## 2        5  weekday 0.38447846
## 3       10  weekday 0.14951940
## 4       15  weekday 0.17087932
## 5       20  weekday 0.08543966
## 6       25  weekday 1.38981844
```

![plot of chunk weekdays](figure/weekdays-1.png)

