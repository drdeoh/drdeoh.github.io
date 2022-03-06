Basic Analysis of data from nycflights13
================

Today, I will be doing basic analysis of data using R.

First, load the following libraries and data.

``` r
library(tidyverse)
library(nycflights13)
```

Look into the data.

``` r
df <- flights
df
```

    ## # A tibble: 336,776 × 19
    ##     year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
    ##    <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
    ##  1  2013     1     1      517            515         2      830            819
    ##  2  2013     1     1      533            529         4      850            830
    ##  3  2013     1     1      542            540         2      923            850
    ##  4  2013     1     1      544            545        -1     1004           1022
    ##  5  2013     1     1      554            600        -6      812            837
    ##  6  2013     1     1      554            558        -4      740            728
    ##  7  2013     1     1      555            600        -5      913            854
    ##  8  2013     1     1      557            600        -3      709            723
    ##  9  2013     1     1      557            600        -3      838            846
    ## 10  2013     1     1      558            600        -2      753            745
    ## # … with 336,766 more rows, and 11 more variables: arr_delay <dbl>,
    ## #   carrier <chr>, flight <int>, tailnum <chr>, origin <chr>, dest <chr>,
    ## #   air_time <dbl>, distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

There is total of 19 columns in the data. Each colums represenets
different variables.

We can use histograms and summary function to analyze the data.

``` r
hist(df$hour, prob = TRUE, main = "Histogram of Hour\nfrom NYC flights 2013", xlab = "Hour")
```

![](Untitled_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

![unnamed-chunk-3-1](https://user-images.githubusercontent.com/93630073/156906649-468eb467-9050-4253-9263-8bf1ab7a4609.png)

To calculate the five summary numbers of the data, use summary function.

``` r
summary(df$hour)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##    1.00    9.00   13.00   13.18   17.00   23.00

The plot is far from Normal distribution shape, but we can add layer of
normal distribution line using mean and sd function.

``` r
hist(df$hour, prob = TRUE, main = "Histogram of Hour\nfrom NYC flights 2013", xlab = "Hour")
xx <- seq(min(df$hour), max(df$hour), length.out = 1000)
yy <- dnorm(xx, mean = mean(df$hour), sd = sd(df$hour))
lines(xx, yy)
```

![](Untitled_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
![unnamed-chunk-5-1](https://user-images.githubusercontent.com/93630073/156906743-c62fa8d7-edd9-4c12-ae5d-9b78b80c124d.png)

We can also filter the data. If we want to extract tail numbers of
flights which flew on June 2nd with UA with larger than 2000km,

``` r
df2 <- df %>%
  filter(carrier == "UA" & month == 6 & day == 2 & distance >= 2000) %>%
  select(tailnum)
df2
```

    ## # A tibble: 54 × 1
    ##    tailnum
    ##    <chr>  
    ##  1 N39463 
    ##  2 N555UA 
    ##  3 N488UA 
    ##  4 N78438 
    ##  5 N475UA 
    ##  6 N76517 
    ##  7 N467UA 
    ##  8 N818UA 
    ##  9 N508UA 
    ## 10 N39423 
    ## # … with 44 more rows

If we want to filter flights in July with departure delay and count,

``` r
df3 <- df %>%
  filter(month == 7 & dep_delay > 0)
count(df3)
```

    ## # A tibble: 1 × 1
    ##       n
    ##   <int>
    ## 1 13909

There were 13909 flights in July 2013 who had departure delay.

To plot histogram of flights in June,

``` r
df4 <- df %>%
  filter(month == 6)
hist(df4$day, prob = TRUE, xlab = "Day", main = "Flights in June 2013")
```

![](Untitled_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->
![unnamed-chunk-8-1](https://user-images.githubusercontent.com/93630073/156906753-9f557131-e182-4193-8eac-ffd393f14e46.png)

This shows there were no significant day with highest or lowest number
of flights.

``` r
df5 <- df %>%
  filter(month == 12)
hist(df5$day, prob = TRUE, main = "Flights in December 12", xlab = "day")
```

![](Untitled_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->
![unnamed-chunk-9-1](https://user-images.githubusercontent.com/93630073/156906761-9e1164f0-80c9-46be-9a21-71aba17ab49c.png)

There is significant less flights at the end of the month of 12. This an
be due to New years eves schedules.

Lastly, let’s create boxplot of distances.

``` r
boxplot(df$distance)
```

![](Untitled_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->
![unnamed-chunk-10-1](https://user-images.githubusercontent.com/93630073/156906777-a7d28f43-c77e-4720-9ebb-0f3612c0b315.png)

``` r
summary(df$distance)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##      17     502     872    1040    1389    4983

There are two significant outliers. Mean is much larger than median
because mean is sensitive to outliers.

Thank you for reading\!\!\!\!\!\!\!\!\!\!\!\!\!\!\!\!\!\!..
