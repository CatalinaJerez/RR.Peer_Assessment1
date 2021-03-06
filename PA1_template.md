---
title: 'Reproducible Research: Peer Assessment 1'
author: "Jerez"
date: "11/11/2020"
output: 
 html_document:
  keep_md: true
---

## Instructions 

This assignment will be described in multiple parts. You will need to write a report that answers the questions detailed below. Ultimately, you will need to complete the entire assignment in a single R markdown document that can be processed by knitr and be transformed into an HTML file.

Throughout your report make sure you always include the code that you used to generate the output you present. When writing code chunks in the R markdown document, always use \color{red}{\verb|echo = TRUE|}echo = TRUE so that someone else will be able to read the code. This assignment will be evaluated via peer assessment so it is essential that your peer evaluators be able to review the code for your analysis.

For the plotting aspects of this assignment, feel free to use any plotting system in R (i.e., base, lattice, ggplot2)

## Library
Call the library that you requires to do the assigment


```r
library(psycho)
```

```
## Note: Many functions of the 'psycho' package have been (improved and) moved to other packages of the new 'easystats' collection (https://github.com/easystats). If you don't find where a function is gone, please open an issue at: https://github.com/easystats/easystats/issues
```

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
library(ggplot2)
```

## Loading and preprocessing the data



```r
# Choose directory
setwd("~/Google Drive/Coursera/RP")

# Call the zip file
if (!file.exists('activity.csv')) {
  unzip(zipfile = "activity.zip")
}

data <- read.csv(file="activity.csv", header=TRUE)

str(data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

```r
names(data)
```

```
## [1] "steps"    "date"     "interval"
```

```r
summary(data)
```

```
##      steps                date          interval     
##  Min.   :  0.00   2012-10-01:  288   Min.   :   0.0  
##  1st Qu.:  0.00   2012-10-02:  288   1st Qu.: 588.8  
##  Median :  0.00   2012-10-03:  288   Median :1177.5  
##  Mean   : 37.38   2012-10-04:  288   Mean   :1177.5  
##  3rd Qu.: 12.00   2012-10-05:  288   3rd Qu.:1766.2  
##  Max.   :806.00   2012-10-06:  288   Max.   :2355.0  
##  NA's   :2304     (Other)   :15840
```

## What is mean total number of steps taken per day?
For this part of the assignment, you can ignore the missing values in the dataset.

1. Calculate the total number of steps taken per day.


```r
total.steps.perDay <- sum(data$steps, na.rm = TRUE)
print(total.steps.perDay)
```

```
## [1] 570608
```

2. If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day.


```r
total.steps.eachDay <- aggregate(steps ~ date, data = data, FUN = sum, na.rm = TRUE)
print(total.steps.eachDay)
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
## 14 2012-10-16 15084
## 15 2012-10-17 13452
## 16 2012-10-18 10056
## 17 2012-10-19 11829
## 18 2012-10-20 10395
## 19 2012-10-21  8821
## 20 2012-10-22 13460
## 21 2012-10-23  8918
## 22 2012-10-24  8355
## 23 2012-10-25  2492
## 24 2012-10-26  6778
## 25 2012-10-27 10119
## 26 2012-10-28 11458
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

```r
hist(total.steps.eachDay$steps, main = 'Total number of step taken each day', xlab = 'Steps', col = 'grey')
abline(v = mean(total.steps.eachDay$steps), col = 'green', lty = 1) # aggregate the mean of total steps
abline(v = median(total.steps.eachDay$steps), col = 'red', lty = 2) # aggregate the median of total steps 
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


3. Calculate and report the mean and median of the total number of steps taken per day


```r
mean.total.steps.eachDay   <- mean(total.steps.eachDay$steps) # mean 
median.total.steps.eachDay <- median(total.steps.eachDay$steps) # median

paste('The mean of the total steps each day is', round(mean.total.steps.eachDay, 2))
```

```
## [1] "The mean of the total steps each day is 10766.19"
```

```r
paste('The median of the total steps each day is', round(median.total.steps.eachDay, 2))
```

```
## [1] "The median of the total steps each day is 10765"
```


## What is the average daily activity pattern?

1. Make a time series plot (i.e. \color{red}{\verb|type = "l"|}type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
Five.Inter <- aggregate(steps ~ interval, data = data, FUN = mean, na.rm = TRUE)

ggplot(Five.Inter, aes(interval, steps))+
 geom_line(col = 'black', lty = 1)+
 labs(title = 'Average daily activity pattern', x = '5-minute interval', y = 'Average number of steps taken')+
 theme_bw(base_family = 'Times')
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
# We use the function which
max.FiveInter <- Five.Inter[which.max(Five.Inter$steps), ]

print(max.FiveInter)
```

```
##     interval    steps
## 104      835 206.1698
```

```r
paste('The interval that contains the largest number of steps is the', max.FiveInter$interval, 'interval with', max.FiveInter$steps, 'step numbers')
```

```
## [1] "The interval that contains the largest number of steps is the 835 interval with 206.169811320755 step numbers"
```


## Imputing missing values

Note that there are a number of days/intervals where there are missing values (coded as \color{red}{\verb|NA|}NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with \color{red}{\verb|NA|}NAs)


```r
NA.values <- is.na(data$steps) # Look the NA's
table(NA.values)   # How many values are in the data
```

```
## NA.values
## FALSE  TRUE 
## 15264  2304
```

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.


```r
data2 <- data
for (i  in 1:length(data$steps)) {
 if(is.na(data2$steps[i])){
  aux <- data2$interval[i]
  for (j in 1:length(Five.Inter$interval)) {
   if(Five.Inter$interval[j] == aux)
    data2$steps[i] <- Five.Inter$steps[j]
  }}}


NA.values2 <- is.na(data2$steps)
table(NA.values2) # Compare this number whit NA.values to confirm that no longer NA in the data
```

```
## NA.values2
## FALSE 
## 17568
```
3. Create a new dataset that is equal to the original dataset but with the missing data filled in.


```r
Five.Inter2 <- aggregate(steps ~ date, data = data2, FUN = sum, na.rm = TRUE)
print(Five.Inter2)
```

```
##          date    steps
## 1  2012-10-01 10766.19
## 2  2012-10-02   126.00
## 3  2012-10-03 11352.00
## 4  2012-10-04 12116.00
## 5  2012-10-05 13294.00
## 6  2012-10-06 15420.00
## 7  2012-10-07 11015.00
## 8  2012-10-08 10766.19
## 9  2012-10-09 12811.00
## 10 2012-10-10  9900.00
## 11 2012-10-11 10304.00
## 12 2012-10-12 17382.00
## 13 2012-10-13 12426.00
## 14 2012-10-14 15098.00
## 15 2012-10-15 10139.00
## 16 2012-10-16 15084.00
## 17 2012-10-17 13452.00
## 18 2012-10-18 10056.00
## 19 2012-10-19 11829.00
## 20 2012-10-20 10395.00
## 21 2012-10-21  8821.00
## 22 2012-10-22 13460.00
## 23 2012-10-23  8918.00
## 24 2012-10-24  8355.00
## 25 2012-10-25  2492.00
## 26 2012-10-26  6778.00
## 27 2012-10-27 10119.00
## 28 2012-10-28 11458.00
## 29 2012-10-29  5018.00
## 30 2012-10-30  9819.00
## 31 2012-10-31 15414.00
## 32 2012-11-01 10766.19
## 33 2012-11-02 10600.00
## 34 2012-11-03 10571.00
## 35 2012-11-04 10766.19
## 36 2012-11-05 10439.00
## 37 2012-11-06  8334.00
## 38 2012-11-07 12883.00
## 39 2012-11-08  3219.00
## 40 2012-11-09 10766.19
## 41 2012-11-10 10766.19
## 42 2012-11-11 12608.00
## 43 2012-11-12 10765.00
## 44 2012-11-13  7336.00
## 45 2012-11-14 10766.19
## 46 2012-11-15    41.00
## 47 2012-11-16  5441.00
## 48 2012-11-17 14339.00
## 49 2012-11-18 15110.00
## 50 2012-11-19  8841.00
## 51 2012-11-20  4472.00
## 52 2012-11-21 12787.00
## 53 2012-11-22 20427.00
## 54 2012-11-23 21194.00
## 55 2012-11-24 14478.00
## 56 2012-11-25 11834.00
## 57 2012-11-26 11162.00
## 58 2012-11-27 13646.00
## 59 2012-11-28 10183.00
## 60 2012-11-29  7047.00
## 61 2012-11-30 10766.19
```

4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


```r
hist(Five.Inter2$steps, main = 'Imputed missing values - Total number of step taken per day', xlab = 'Steps', col = 'grey')
abline(v = mean(Five.Inter2$steps), col = 'green', lty = 1) # aggregate the mean of total steps
abline(v = median(Five.Inter2$steps), col = 'red', lty = 2) # aggregate the median of total steps
```

![](PA1_template_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


```r
mean.Five.Inter2   <- mean(Five.Inter2$steps) # mean 
median.Five.Inter2 <- median(Five.Inter2$steps) # median

diff.Mean   <- mean.total.steps.eachDay - mean.Five.Inter2
diff.Median <- median.total.steps.eachDay - median.Five.Inter2

paste('The mean of the total steps each day is', round(mean.Five.Inter2, 2))
```

```
## [1] "The mean of the total steps each day is 10766.19"
```

```r
paste('The median of the total steps each day is', round(median.Five.Inter2, 2))
```

```
## [1] "The median of the total steps each day is 10766.19"
```

```r
paste('The differences into mean and median are:', round(diff.Mean, 2), '&', round(diff.Median))
```

```
## [1] "The differences into mean and median are: 0 & -1"
```

## Are there differences in activity patterns between weekdays and weekends?

For this part the \color{red}{\verb|weekdays()|}weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

```r
# We create a function to evaluate which days is our interest 

Week.days <- function(date) {
  day <- weekdays(date) # use function weekdays()
  if (day %in% c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'))
      return ("Weekday")
  else if (day %in% c('Saturday', 'Sunday'))
      return ("Weekend")
  else
      stop ("Format date unknown")
}
```

Creating the new factor...


```r
data2$date <- as.Date(data2$date)
data2$day  <- sapply(data2$date, FUN = Week.days)

str(data2)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ steps   : num  1.717 0.3396 0.1321 0.1509 0.0755 ...
##  $ date    : Date, format: "2012-10-01" "2012-10-01" ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ day     : chr  "Weekday" "Weekday" "Weekday" "Weekday" ...
```

2. Make a panel plot containing a time series plot (i.e. \color{red}{\verb|type = "l"|}type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
Intervals.week <- aggregate(steps ~ interval + day, data2, mean) # Create the data, you have to considerer the days.

ggplot(Intervals.week, aes(interval, steps))+
 geom_line(lty = 1, aes(color = day))+
 facet_grid(day ~ .)+
 labs(title = 'Average daily activity pattern', x = '5-minute interval', y = 'Average number of steps taken')+
 theme_bw(base_family = 'Times')+
 guides(color = 'none')
```

![](PA1_template_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

