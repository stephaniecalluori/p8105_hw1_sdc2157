p8105_hw1_sdc2157
================
Stephanie Calluori
2023-09-23

``` r
library(moderndive)
library(tidyverse)
```

# Problem 1

## Describing our data

We will examine the early_january_weather dataset, which catalogs hourly
weather data for LGA, JFK, and EWR airports during January 2013.

``` r
data("early_january_weather")
```

The data set contains 358 observations and 15 variables. Variables
include origin, year, month, day, hour, temp, dewp, humid, wind_dir,
wind_speed, wind_gust, precip, pressure, visib, time_hour .

The average temperature was 39.6 degrees F with a standard deviation of
7.1 degrees F. The minimum and maximum temperatures were 24.1 and 57.9
degrees F, respectively.

Some additional values of note include average wind speed at 8.2 mph,
and average visibility at 8.5 miles.

## Time for plotting!

``` r
ggplot(early_january_weather, aes(x = time_hour, y = temp, color = humid)) + 
  geom_point() +
  labs(y = "Temperature (F)", 
       x = "Time of Recording (Hourly)", 
       title = "Hourly Temperature and Humidity during Jan 2013")
```

![](template_files/figure-gfm/scatterplot-1.png)<!-- -->

``` r
ggsave("scatterplot.pdf")
```

Figure 1: Hourly Temperature and Humidity during Jan 2013. While regular
fluctuations in temperature occurred, an overall increase in temperature
was observed from early to mid-January. Relative humidity was low, then
considerably increased near mid-January.

# Problem 2

## Exploring Variable Types

First, we will create a data frame.

``` r
set.seed(10)

example_df <-
  tibble(
    vec_num = rnorm(10),
    vec_logical = (vec_num > 0),
    vec_char = c("a", "b", "c", "d", "e", "f", "g", "h", "i", "j"),
    vec_factor = factor(c("small", "small", "medium", "large", "medium", "large", 
                          "small", "large", "large", "large"),
                        levels = c("small", "medium", "large")
                        )
  )
```

We will attempt to take the mean of each variable in our data frame.

``` r
n_avg <- mean(
  pull(example_df, vec_num)
  )

log_avg <- mean(
  pull(example_df, vec_logical)
  )

char_avg <- mean(
  pull(example_df, vec_char)
  )
```

    ## Warning in mean.default(pull(example_df, vec_char)): argument is not numeric or
    ## logical: returning NA

``` r
fac_avg <- mean(
  pull(example_df, vec_factor)
  )
```

    ## Warning in mean.default(pull(example_df, vec_factor)): argument is not numeric
    ## or logical: returning NA

Notice the warning messages that appear. You can only take the mean of
numeric and logical variables.

The mean of the numeric variable `vec_num` outputs -0.4906568,
representing the arithmetic mean. The mean of the logical variable
`vec_logical` outputs 0.3, representing the average number of `TRUE`
values (under the hood, `TRUE == 1`). The mean of the character variable
`vec_char` and the factor variable `vec_factor` output NA and NA,
respectively, since there are no numeric values or numeric equivalents
to compute.

## Converting Variable Types

We can convert non-numeric variables to numeric variables.

``` r
vec_log_num <- as.numeric(
  pull(example_df, vec_logical)
  )

vec_char_num <- as.numeric(
  pull(example_df, vec_char)
  )

vec_factor_num <- as.numeric(
  pull(example_df, vec_factor)
  )
```

- Converting Logical to Numeric: Explicitly converts `TRUE` to 1 and
  `FALSE` to 0.
- Converting Character to Numeric: Each character value is converted to
  `NA`.
- Converting Factor to Numeric: Each factor value is converted to either
  1, 2, or 3, which represent the three factor levels we previously
  defined.

Now, let’s try to take the mean of each of our converted variables.

``` r
mean(vec_log_num)
```

    ## [1] 0.3

``` r
mean(vec_char_num)
```

    ## [1] NA

``` r
mean(vec_factor_num)
```

    ## [1] 2.2

As previously described, output for the mean of `vec_log_num` represents
the average number of `TRUE` values. Output for the mean of
`vec_char_num` remains `NA` as there are no numerical values that
correspond to the `NA` values. Output for the mean of `vec_factor_num`
now represents the average value of the factor levels.
