# <center> <h1>Differences in activity paterns between weekdays and weekends</center>
  
## Introduction

It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a [Fitbit][1], [Nike Fuelband][2], or [Jawbone Up][3]. These type of devices are part of the "quantified self" movement -- a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

We were asked to look at some data collected by one of these devices.  The device collected data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and includes the number of steps taken in 5 minute intervals each day. There were four questions from this data that were to be answered.

1. What is the total number of steps taken perday?   
2. What is the average daily activity pattern?   
3. Does the missing data (NAs) effect median or mean of the data?   
4. Are ther differences in activity between weekdays and weekends?



[1]:http://www.fitbit.com/ "Fitbit"
[2]:http://www.nike.com/us/en_us/c/nikeplus-fuelband "Nike Fuelband"
[3]:https://jawbone.com/up "Jawbone Up"

### Collecting the data
The data for this analysis was downloaded from the course website 
url <- "https://d396qusza40orc.cloudfront.net/repdata%Fdata%2Factivity.zip"
The dataset is a comma-separated-value (CSV) file with 17,568 observations and one header row.  The size of the file is 52K.

The variables with in the data set are:

1.  steps: The number of steps.    
    - Measured in 5 minute intervales.   
    - Missing values are coded as NA.
2.  date: The date the measurement was taken.    
    - format (YYY-MM-DD)
3.  interval: Indentifer for the 5-minute interval the measurement was taken.

## Loading and preprocessing the data

```r
#  I did not include the code to download the data from R do to differences in platforms and individual system setups and preferences for downloading data sets. Once your data is downloaded read the data set.
act <- read.csv("activity.csv")
```

## What is mean total number of steps taken per day?
- The mean of the total number of steps taken per day is 10766.19 steps per day.
- The median ot the total number of steps taken per day is 10765 steps per day.


```r
#  Calculate the number of steps taken a day.
 spd <- aggregate(steps~date, act,sum)
#  Get the mean of the steps per day.
 spdmn <- mean(spd$steps)
 spdmn
```

```
## [1] 10766.19
```

```r
# Get the median of the steps per day.
spdmdn <- median(spd$steps)
spdmdn
```

```
## [1] 10765
```
- Histogram of step per day.

```r
# builds the histogram
hist(spd$steps, main = paste("Total Steps Per Day"), col="light blue", ylab= "Number of days Occurred (frequency)", xlab="Number of Steps", breaks=seq(0,22000,2200))
# Plots a vertical line where the mean is located.  Did not plot a seperate line for median because it is .81 less than mean, it will overlay.
abline(v=spdmn, col="red",lwd=4)
# Place mean label on plot.
text(spdmn, 15, paste("Mean = ", round(spdmn, 2)),col = "Red",pos = 4)
# place median label on plot.
text(spdmn, 13, paste("Median = ", round(spdmdn, 2)),col = "Red",pos = 4)
```

![Histogram1](https://github.com/Whockensmith/RepData_PeerAssessment1/blob/master/histo1.png)

## What is the average daily activity pattern?

```r
# Get the mean of the steps by interval (time of each day).
sbi <- aggregate(steps~interval, act,mean)
# Build a plot
plot(sbi$interval,sbi$steps, xlab = "Time Interval", ylab = "Number of Steps", main = "Daily Activity Patter")
# Add a vertical line at the highest time of most steps
Maxint <- sbi[which.max(sbi$steps),1]
Maxint
```

```
## [1] 835
```

```r
abline(v= Maxint,lwd=2,col="red")
text(Maxint,200, paste(Maxint," has the higest average number of steps"),col = "Red",pos = 4)
```

![Box Plot](https://github.com/Whockensmith/RepData_PeerAssessment1/blob/master/histo2.png)


## Imputing missing values



```r
#  Replace all NAs in steps column to the average steps for that time interval from the other days at that time interval.
actfx <- transform(act, steps = ifelse(is.na(act$steps), sbi$steps[match(act$interval, sbi$interval)], act$steps))
spdfx <- aggregate(steps~date, actfx,sum)
spdfxmn <- mean(spdfx$steps)
spdfxmdn <- median(spdfx$steps)
```



```r
hist(spdfx$steps, main = paste("Total Steps Per Day"), col="light blue", ylab= "Number of days Occurred (frequency)", xlab="Number of Steps", breaks=seq(0,22000,2200))
abline(v=spdfxmn, col="red",lwd=4)
# Place mean label on plot.
text(spdfxmn, 22, paste("Mean = ", round(spdfxmn, 2)),col = "Red",pos = 4)
# place median label on plot.
text(spdfxmn, 20, paste("Median = ", round(spdfxmdn, 2)),col = "Red",pos = 4)
```

![Histogram 3](https://github.com/Whockensmith/RepData_PeerAssessment1/blob/master/histo3.png)

## Compairing the two histograms.   
  - First with (NAs) and with (NAs) corrected to averages based off of other days with data.

<img src="Differences_in_activity_paterns_between_weekdays_and_weekends_files/figure-html/unnamed-chunk-1-1.png" style="float:left" />
<img src="Differences_in_activity_paterns_between_weekdays_and_weekends_files/figure-html/unnamed-chunk-2-1.png" style="float:right" />

###### Compairing the two the mean remained the same, but when the (NAs) were fixed the median mached the mean and the frequency of the band the mean is in increased.




## Are there differences in activity patterns between weekdays and weekends?
