---
ttitle: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---
# Reproducible Research - Coursera (Johns Hopkins University)
## Peer Assessment 1

First thing I did was to clone the GitHub repository to my working directory  
and set the new working directory to the folder where the repository is.

## Loading and preprocessing the data

The activity data is in a zipped file so we have to unzip the file before  
reading the data and store it on the variable "activity":


```r
unzip("activity.zip")
activity <- read.csv("activity.csv")
```

I want to know what type of data we are dealing with so I call the str function:


```r
str(activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```

I see that the date column has a Factor format. In order to proceed with my  
analysis it seems convenient to have that data with a date format. 


```r
library(dplyr)
```

```
## Error: package or namespace load failed for 'dplyr' in loadNamespace(i, c(lib.loc, .libPaths()), versionCheck = vI[[i]]):
##  there is no package called 'DBI'
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
activity1 <- mutate(activity, date = ymd(date))
```

```
## Error in mutate(activity, date = ymd(date)): could not find function "mutate"
```

## What is mean total number of steps taken per day?

I want to calculate the total number of steps taken by day and make a histogram 
with that data. I will calculate the sum of steps by first grouping the data by 
date and then calculate the total using the sum() :


```r
tot.steps <- activity1 %>% 
        group_by(date) %>% 
        summarize(steps = sum(steps, na.rm = T))
```

```
## Error in activity1 %>% group_by(date) %>% summarize(steps = sum(steps, : could not find function "%>%"
```

The assessment asks for a histogram and not a barplot. A histogram is a  
representation of frequency but what I have is a table with dates and number  
of steps, with some data missing (NA). So first I will create a data frame  
with just the data not containing NAs. After I will create a variable called  
steps that will contain data objects repeated as many times as steps were  
given on that particular day to finally create a histogram with that data:


```r
tot.steps1 <- tot.steps[complete.cases(tot.steps),]
```

```
## Error in eval(expr, envir, enclos): object 'tot.steps' not found
```

```r
steps <- as.Date(rep(tot.steps1$date, tot.steps1$steps))
```

```
## Error in as.Date(rep(tot.steps1$date, tot.steps1$steps)): object 'tot.steps1' not found
```

```r
hist(steps, "days", freq = T, main = "Total steps per day", xlab = "Date",
     ylab = "Total step number", xlim = range(tot.steps$date))
```

```
## Error in hist(steps, "days", freq = T, main = "Total steps per day", xlab = "Date", : object 'steps' not found
```
I also want to calculate the mean and the median of the total number of steps  
taken per day.


```r
median <- median(tot.steps$steps, na.rm = T)
```

```
## Error in median(tot.steps$steps, na.rm = T): object 'tot.steps' not found
```

```r
mean <- mean(tot.steps$steps, na.rm = T)
```

```
## Error in mean(tot.steps$steps, na.rm = T): object 'tot.steps' not found
```



















