---
title: "Reproducible Reserach Project 1"
author: "Toshiyuki Tachibana"
date: '2016-12-14'
output: html_document keep_md=TRUE
---

## Introduction

=====================

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals throughout the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

This document presents the results of the Reproducible Research's Project 1 in a report using a single R markdown document that can be processed by knitr and be transformed into an HTML file. 

## Prepare R environment

Thoughout this report I iclude the code that I used to generate the output my present. WHen writing code chunks in the R markdown document, always use <span style="color:red">echo = TRUE</span> so that someone else will be able to read rge code.


```r
library(knitr)
opts_chunk$set(echo = TRUE)
```


## Load necessary libraries


```r
library(dplyr)
library(lubridate)
library(ggplot2)
```

##Loading and preprocessing the data

=============================================================

Show any code that is needed to

1.Load the data (i.e.<span style="color:red">read.csv()</span>)

2.Process/transform the data (if necessary) into a format suitable for your analysis

## Download, unzip and read the data into dataframe


```r
if(!file.exists("getdata-projectfiles-UCI HAR Dataset.zip")) {
        temp <- tempfile()
        download.file("http://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip",temp)
        unzip(temp)
        unlink(temp)
}

data <- read.csv("activity.csv")
```

## Process/transform the data into a format suitable for my analysis


```r
data$date <- ymd(data$date)
```


## Check the data with str() and head()


```r
str(data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Date, format: "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```


```r
head(data)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```

## What is mean total number of steps taken per day?

====================================================================================

For this part of the assignment the missing values can be ignored.

1.Calculate the total number of steps taken per day.

2.Make a histogram of the total number of steps taken each day.

3.Calculate and report the mean and median of the total number of steps taken per day.

##Sum steps per day, create Histogram, and calculate mean and median

## Calculate sum steps per day

```r
steps_by_day <- aggregate(steps ~ date, data, sum)
```


```r
steps_by_day
```

```
##          date steps
## 1  2012-10-02   126
## 2  2012-10-03 11352
## 3  2012-10-04 12116
## 4  2012-10-05 13294
## 5  2012-10-06 15420
## 6  2012-10-07 11015
## 7  2012-10-09 12811
## 8  2012-10-10  9900
## 9  2012-10-11 10304
## 10 2012-10-12 17382
## 11 2012-10-13 12426
## 12 2012-10-14 15098
## 13 2012-10-15 10139
## 14 2012-10-16 15084## 15 2012-10-17 13452
## 16 2012-10-18 10056
## 17 2012-10-19 11829
## 18 2012-10-20 10395
## 19 2012-10-21  8821
## 20 2012-10-22 13460
## 21 2012-10-23  8918
## 22 2012-10-24  8355## 23 2012-10-25  2492
## 24 2012-10-26  6778
## 25 2012-10-27 10119## 26 2012-10-28 11458
## 27 2012-10-29  5018
## 28 2012-10-30  9819
## 29 2012-10-31 15414
## 30 2012-11-02 10600
## 31 2012-11-03 10571
## 32 2012-11-05 10439
## 33 2012-11-06  8334
## 34 2012-11-07 12883
## 35 2012-11-08  3219
## 36 2012-11-11 12608
## 37 2012-11-12 10765
## 38 2012-11-13  7336
## 39 2012-11-15    41
## 40 2012-11-16  5441
## 41 2012-11-17 14339
## 42 2012-11-18 15110
## 43 2012-11-19  8841
## 44 2012-11-20  4472
## 45 2012-11-21 12787
## 46 2012-11-22 20427
## 47 2012-11-23 21194
## 48 2012-11-24 14478
## 49 2012-11-25 11834
## 50 2012-11-26 11162
## 51 2012-11-27 13646
## 52 2012-11-28 10183
## 53 2012-11-29  7047
```


# Histgram of total steps by day

```r
hist(steps_by_day$steps, main = paste("Total Steps Each Day"), col="blue", xlab="Number of Steps")
```

![alt tag](https://github.com/toshitachi/RepData_PeerAssessment1/blob/master/figures/Total%20steps%20by%20day.png )

```r
step_by_day_mean <- mean(steps_by_day$steps)
step_by_day_median <- median(steps_by_day$steps)
```

```r
step_by_day_mean
```

```
## [1] 10766.19
```
Mean of total steps per day is 10766.19.

```r
step_by_day_median
```

```
## [1] 10765
```
Median of total steps per day is 10765.

## What is the average daily activity pattern?

=======================================================================

1.Make a time series plot (i.e.<span style="color:red">type = "l"</span>) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

2.Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

## Calculate average steps for each interval for all days


```r
steps_by_interval <- aggregate(steps ~ interval, data, mean)
```

## Plot average steps for each interval


```r
plot(steps_by_interval$interval,steps_by_interval$steps, type="l", xlab="Interval", ylab="Number of Steps",main="Average Number of Steps per Day by Interval")
```

![alt tag](https://github.com/toshitachi/RepData_PeerAssessment1/blob/master/figures/Avarage%20steps%20for%20each%20interval.png)

## Use which.max() to find out the maximum steps, on average, across all the days


```r
steps_by_interval[which.max(steps_by_interval$steps),]
```

```
##     interval    steps
## 104      835 206.1698
```

The interval 835 has, on average, the highest count of steps, with 206 steps.

## Imputing missing values

==========================================

Note that there are a number of days/intervals where there are missing values (coded as <span.type="color:red">NA</span>). The presence of missing days may introduce bias into some calculations or summaries of the data.

1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with <span style="color:red">NAs</span>)

2.Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

3.Create a new dataset that is equal to the original dataset but with the missing data filled in.

4.Make a histogram of the total number of steps taken each day and Calculate and report the **mean** and **median** total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

## Calculate total number of missing values


```r
sum(is.na(data$steps))
```

```
## [1] 2304
```

## Devise a strategy for filling in all of the missing values in the dataset.
Simply apply the mean as the missing values of <span style="color:red">Nas</span>.


```r
data_full <- data
nas <- is.na(data_full$steps)
avg_interval <- tapply(data_full$steps, data_full$interval, mean, na.rm=TRUE, simplify=TRUE)
data_full$steps[nas] <- avg_interval[as.character(data_full$interval[nas])]
```

## Check that there are no missing values


```r
sum(is.na(data_full$steps))
```

```
## [1] 0
```

## Recount total steps by day 


```r
steps_by_day_i <- aggregate(steps ~ date, data_full, sum)
```


```r
head(steps_by_day_i)
```

```
##         date    steps
## 1 2012-10-01 10766.19
## 2 2012-10-02   126.00
## 3 2012-10-03 11352.00
## 4 2012-10-04 12116.00
## 5 2012-10-05 13294.00
## 6 2012-10-06 15420.00
```

## Plot histgram of total steps by day where NAs are filled with the mean


```r
hist(steps_by_day_i$steps, main = paste("Total Steps Each Day"), col="blue", xlab="Number of Steps")
```

![alt tag](https://github.com/toshitachi/RepData_PeerAssessment1/blob/master/figures/Total%20ssteps%20by%20day%20where%20NAs%20are%20filled%20with%20the%20mean.png)

## Calculate the mean and median steps with the filled in values 


```r
rmean.i <- mean(steps_by_day_i$steps)
rmedian.i <- median(steps_by_day_i$steps)
```


```r
rmean.i
```

```
## [1] 10766.19
```

```r
rmedian.i
```

```
## [1] 10766.19
```
Mean and Median of imputed data are 10766.19.

## Are there differences in activity patterns between 

===================================================================================

## weekdays and weekends?

==============================================

For this part the <span style="color:red">weekdays()</span> function may be of some help here. Use the dataset with the filled-in missing values for this part.

1.Create a new factor variable in the dataset with two levels 窶? 窶忤eekday窶? and 窶忤eekend窶? indicating whether a given date is a weekday or weekend day.

2.Make a panel plot containing a time series plot (i.e. <span style="color:red">type = "l"</span>) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

## Use dplyr and mutate to create a new column, weektype, and apply whether the day is weekend or weekday


```r
steps_by_interval_i<- mutate(data_full, weektype = ifelse(weekdays(data_full$date) == "土曜日" | weekdays(data_full$date) == "日曜日", "weekend", "weekday"))
```


```r
head(steps_by_interval_i)
```

```
##       steps       date interval weektype
## 1 1.7169811 2012-10-01        0  weekday
## 2 0.3396226 2012-10-01        5  weekday
## 3 0.1320755 2012-10-01       10  weekday
## 4 0.1509434 2012-10-01       15  weekday
## 5 0.0754717 2012-10-01       20  weekday
## 6 2.0943396 2012-10-01       25  weekday
```

## Make a panel plot containing a time series plot (i.e. <span style="color:red">type = "l"</span>) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). 


```r
steps_by_interval_i<- aggregate(steps ~ interval+weektype,steps_by_interval_i, mean)
```


```r
ggplot(steps_by_interval_i, aes(interval, steps,color = weektype )) + 
    geom_line() + 
    facet_grid(weektype ~ .) +
    xlab("5-minute interval") + 
    ylab("avarage number of steps")
```

![alt tag](https://github.com/toshitachi/RepData_PeerAssessment1/blob/master/figures/Interval%20and%20average%20steps%20by%20weektype.png)
