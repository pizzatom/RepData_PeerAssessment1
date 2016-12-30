# Reproducible Research: Peer Assessment 1

## Source packages 
Install them if needed (TODO)

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```


## Loading and preprocessing the data
Assume data is in the same directory as this script

1. If no csv, unzip first
2. Read csv 
3. Format date


```r
if (!any(grepl("activity.csv",list.files()))){
  unzip("activity.zip")
}

df <- read.csv("activity.csv")
```



## What is mean total number of steps taken per day?
Summarize steps per day with the dplyr package. Ignore missing data. 

1. Average daily steps
  a. group_by date 
  b. summarize dates using sum()
  c. summarize daily means using mean()


```r
avg_daily_steps <-  group_by(df, date) %>% 
                    summarize(daily = sum(steps, na.rm=TRUE)) %>%
                    summarize(mean_daily = mean(daily, na.rm = TRUE))
print(avg_daily_steps)
```

```
## # A tibble: 1 × 1
##   mean_daily
##        <dbl>
## 1    9354.23
```

2. Median daily steps
  a. group_by date 
  b. summarize dates using sum()
  c. summarize daily means using medain()
  

```r
med_daily_steps <-  group_by(df, date) %>% 
                    summarize(daily = sum(steps, na.rm=TRUE)) %>%
                    summarize(median_daily = median(daily, na.rm = TRUE))
print(med_daily_steps)
```

```
## # A tibble: 1 × 1
##   median_daily
##          <int>
## 1        10395
```


## What is the average daily activity pattern?

1. Create a time series plot of the averaged 5-minute activity patterns using the ggplot2 package.
  a. group_by interval
  b. summarize dates using mean(), ignoring missing data for now
  c. plot (type = "l")
  

```r
avg_interval_steps <- group_by(df, interval) %>%
                      summarize(mean_steps = mean(steps, na.rm=TRUE))
f <- ggplot(avg_interval_steps, aes(interval, mean_steps))
f <- f + geom_line()
f
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

2. Find the 5 minute interval with the highest average step count. Add it to the plot for fun. 


```r
highest_avg_interval <- avg_interval_steps[which.max(avg_interval_steps$mean_steps), ]
print(highest_avg_interval)
```

```
## # A tibble: 1 × 2
##   interval mean_steps
##      <int>      <dbl>
## 1      835   206.1698
```

```r
f <- f + geom_point(data = highest_avg_interval, shape = 11 , size = 4, color = "red")
f
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
