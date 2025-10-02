Exploratory Data Analysis
================
2025-10-02

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.2     ✔ tibble    3.3.0
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.1.0     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(p8105.datasets)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

Import Weather Data

``` r
data("weather_df")


weather_df = 
  weather_df |> 
  mutate(month = floor_date(date, unit = "month")) 


#floor(date() --> this takes the date and makes it a month. Ex.Feb 21st becomes Feb 1st 
#Look into package lubridate --> all about how to work with dates
```

Make plots

``` r
weather_df |> 
  ggplot(aes(x=prcp)) +
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 15 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

<img src="eda_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

``` r
#right off the bat we can see some extreme values.
```

First thing, check extreme values

``` r
weather_df |> 
  filter(prcp >1000)
```

    ## # A tibble: 3 × 7
    ##   name           id          date        prcp  tmax  tmin month     
    ##   <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>    
    ## 1 CentralPark_NY USW00094728 2021-08-21  1130  27.8  22.8 2021-08-01
    ## 2 CentralPark_NY USW00094728 2021-09-01  1811  25.6  17.2 2021-09-01
    ## 3 Molokai_HI     USW00022534 2022-12-18  1120  23.3  18.9 2022-12-01

Look at data again

``` r
weather_df |> 
  filter(tmax >= 20, tmax <= 30) |> 
  ggplot(aes( x = tmin, y = tmax, color = name)) +
  geom_point()
```

<img src="eda_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
#check out why waterhole is looking different than Malakai and NYC? It is because of how they got their temp.
```

Group by name

``` r
weather_df |> 
  group_by(name)
```

    ## # A tibble: 2,190 × 7
    ## # Groups:   name [3]
    ##    name           id          date        prcp  tmax  tmin month     
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>    
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01
    ## # ℹ 2,180 more rows

``` r
#gives three distinct groups

weather_df |> 
  group_by(name, month)
```

    ## # A tibble: 2,190 × 7
    ## # Groups:   name, month [72]
    ##    name           id          date        prcp  tmax  tmin month     
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>    
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01
    ## # ℹ 2,180 more rows

``` r
#gives 72 becuase 12 months and 3 groups 
```

Sumarize groups

``` r
weather_df |> 
  group_by(name) |> 
  summarize(
    n = n()
  )
```

    ## # A tibble: 3 × 2
    ##   name               n
    ##   <chr>          <int>
    ## 1 CentralPark_NY   730
    ## 2 Molokai_HI       730
    ## 3 Waterhole_WA     730

``` r
#prints out tibble



weather_df |> 
  group_by(name, month) |> 
  summarize(
    n = n()
  )
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

    ## # A tibble: 72 × 3
    ## # Groups:   name [3]
    ##    name           month          n
    ##    <chr>          <date>     <int>
    ##  1 CentralPark_NY 2021-01-01    31
    ##  2 CentralPark_NY 2021-02-01    28
    ##  3 CentralPark_NY 2021-03-01    31
    ##  4 CentralPark_NY 2021-04-01    30
    ##  5 CentralPark_NY 2021-05-01    31
    ##  6 CentralPark_NY 2021-06-01    30
    ##  7 CentralPark_NY 2021-07-01    31
    ##  8 CentralPark_NY 2021-08-01    31
    ##  9 CentralPark_NY 2021-09-01    30
    ## 10 CentralPark_NY 2021-10-01    31
    ## # ℹ 62 more rows

``` r
#n tells us how many days are in each month here


#distinct instances
weather_df |> 
  group_by(month) |> 
  summarise(
    n = n_distinct(date)
  )
```

    ## # A tibble: 24 × 2
    ##    month          n
    ##    <date>     <int>
    ##  1 2021-01-01    31
    ##  2 2021-02-01    28
    ##  3 2021-03-01    31
    ##  4 2021-04-01    30
    ##  5 2021-05-01    31
    ##  6 2021-06-01    30
    ##  7 2021-07-01    31
    ##  8 2021-08-01    31
    ##  9 2021-09-01    30
    ## 10 2021-10-01    31
    ## # ℹ 14 more rows

Special function to count directly

``` r
weather_df |> 
  count(name) #short hand that says group by name and count by group
```

    ## # A tibble: 3 × 2
    ##   name               n
    ##   <chr>          <int>
    ## 1 CentralPark_NY   730
    ## 2 Molokai_HI       730
    ## 3 Waterhole_WA     730

``` r
weather_df |> 
  group_by(name, month) |> 
  summarise(
    mean_tmax = mean(tmax, na.rm = TRUE),
    median_tmin = median(tmin, na.rm = TRUE),
    sd_prcp = sd(prcp, na.rm = TRUE)
  ) |> view()
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

This is still a dataframe

``` r
weather_df |> 
  group_by(name, month) |> 
  summarise(
    mean_tmax = mean(tmax, na.rm = TRUE)
    ) |> 
  ggplot(aes(x = month, y = mean_tmax, color = name)) +
  geom_point() +
  geom_line()
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

<img src="eda_files/figure-gfm/unnamed-chunk-10-1.png" width="90%" />

Sometimes you need to format results more nicely

``` r
weather_df |> 
  group_by(name, month) |> 
  summarise(
    mean_tmax = mean(tmax, na.rm = TRUE)
  ) |> 
  pivot_wider(
    names_from = name,
    values_from = mean_tmax
  ) |> 
  knitr::kable(digits = 2)
```

    ## `summarise()` has grouped output by 'name'. You can override using the
    ## `.groups` argument.

| month      | CentralPark_NY | Molokai_HI | Waterhole_WA |
|:-----------|---------------:|-----------:|-------------:|
| 2021-01-01 |           4.27 |      27.62 |         0.80 |
| 2021-02-01 |           3.87 |      26.37 |        -0.79 |
| 2021-03-01 |          12.29 |      25.86 |         2.62 |
| 2021-04-01 |          17.61 |      26.57 |         6.10 |
| 2021-05-01 |          22.08 |      28.58 |         8.20 |
| 2021-06-01 |          28.06 |      29.59 |        15.25 |
| 2021-07-01 |          28.35 |      29.99 |        17.34 |
| 2021-08-01 |          28.81 |      29.52 |        17.15 |
| 2021-09-01 |          24.79 |      29.67 |        12.65 |
| 2021-10-01 |          19.93 |      29.13 |         5.48 |
| 2021-11-01 |          11.54 |      28.85 |         3.53 |
| 2021-12-01 |           9.59 |      26.19 |        -2.10 |
| 2022-01-01 |           2.85 |      26.61 |         3.61 |
| 2022-02-01 |           7.65 |      26.83 |         2.99 |
| 2022-03-01 |          11.99 |      27.73 |         3.42 |
| 2022-04-01 |          15.81 |      27.72 |         2.46 |
| 2022-05-01 |          22.25 |      28.28 |         5.81 |
| 2022-06-01 |          26.09 |      29.16 |        11.13 |
| 2022-07-01 |          30.72 |      29.53 |        15.86 |
| 2022-08-01 |          30.50 |      30.70 |        18.83 |
| 2022-09-01 |          24.92 |      30.41 |        15.21 |
| 2022-10-01 |          17.43 |      29.22 |        11.88 |
| 2022-11-01 |          14.02 |      27.96 |         2.14 |
| 2022-12-01 |           6.76 |      27.35 |        -0.46 |

# Mutate with groups

``` r
weather_df |> 
  group_by(name) |> 
  mutate(
    mean_tmax = mean(tmax, na.rm = TRUE),
    center_tmax = tmax - mean_tmax
  ) |> 
  ggplot(aes(x = date, y = center_tmax, color = name)) +
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

<img src="eda_files/figure-gfm/unnamed-chunk-12-1.png" width="90%" />

``` r
#we are standardizing --> shifting our means so they equal zero 
```

Look for cold days

``` r
weather_df |> 
  group_by(name, month) |> 
  mutate(temp_rank = min_rank(desc(tmin))) |> 
  filter(temp_rank < 2)
```

    ## # A tibble: 98 × 8
    ## # Groups:   name, month [72]
    ##    name           id          date        prcp  tmax  tmin month      temp_rank
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>         <int>
    ##  1 CentralPark_NY USW00094728 2021-01-14     0   9.4   3.9 2021-01-01         1
    ##  2 CentralPark_NY USW00094728 2021-02-28    56   7.2   5   2021-02-01         1
    ##  3 CentralPark_NY USW00094728 2021-03-12     0  20    11.1 2021-03-01         1
    ##  4 CentralPark_NY USW00094728 2021-03-26    48  27.8  11.1 2021-03-01         1
    ##  5 CentralPark_NY USW00094728 2021-04-29     5  23.9  15   2021-04-01         1
    ##  6 CentralPark_NY USW00094728 2021-05-23     0  31.1  20   2021-05-01         1
    ##  7 CentralPark_NY USW00094728 2021-06-29     0  35    25.6 2021-06-01         1
    ##  8 CentralPark_NY USW00094728 2021-07-16     0  32.8  26.1 2021-07-01         1
    ##  9 CentralPark_NY USW00094728 2021-08-13     0  34.4  25.6 2021-08-01         1
    ## 10 CentralPark_NY USW00094728 2021-08-26     0  32.8  25.6 2021-08-01         1
    ## # ℹ 88 more rows

``` r
#without filtering every day would be ranked by tmin
weather_df |> 
  group_by(name, month) |> 
  mutate(temp_rank = min_rank(tmin))
```

    ## # A tibble: 2,190 × 8
    ## # Groups:   name, month [72]
    ##    name           id          date        prcp  tmax  tmin month      temp_rank
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>         <int>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 2021-01-01        17
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 2021-01-01        25
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 2021-01-01        19
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 2021-01-01        21
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 2021-01-01        25
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 2021-01-01        19
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   2021-01-01        14
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 2021-01-01        10
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 2021-01-01         7
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 2021-01-01        13
    ## # ℹ 2,180 more rows

What about lags?

``` r
weather_df |> 
  mutate(lagged_tmax = lag(tmax))
```

    ## # A tibble: 2,190 × 8
    ##    name           id         date        prcp  tmax  tmin month      lagged_tmax
    ##    <chr>          <chr>      <date>     <dbl> <dbl> <dbl> <date>           <dbl>
    ##  1 CentralPark_NY USW000947… 2021-01-01   157   4.4   0.6 2021-01-01        NA  
    ##  2 CentralPark_NY USW000947… 2021-01-02    13  10.6   2.2 2021-01-01         4.4
    ##  3 CentralPark_NY USW000947… 2021-01-03    56   3.3   1.1 2021-01-01        10.6
    ##  4 CentralPark_NY USW000947… 2021-01-04     5   6.1   1.7 2021-01-01         3.3
    ##  5 CentralPark_NY USW000947… 2021-01-05     0   5.6   2.2 2021-01-01         6.1
    ##  6 CentralPark_NY USW000947… 2021-01-06     0   5     1.1 2021-01-01         5.6
    ##  7 CentralPark_NY USW000947… 2021-01-07     0   5    -1   2021-01-01         5  
    ##  8 CentralPark_NY USW000947… 2021-01-08     0   2.8  -2.7 2021-01-01         5  
    ##  9 CentralPark_NY USW000947… 2021-01-09     0   2.8  -4.3 2021-01-01         2.8
    ## 10 CentralPark_NY USW000947… 2021-01-10     0   5    -1.6 2021-01-01         2.8
    ## # ℹ 2,180 more rows

``` r
#current form is not taking into account that we have different locations, grouping fixes this
#by default lag() is 1. what is the previous or next value you are interested in 


weather_df |> 
  group_by(name) |> 
  mutate(lagged_tmax = lag(tmax))
```

    ## # A tibble: 2,190 × 8
    ## # Groups:   name [3]
    ##    name           id         date        prcp  tmax  tmin month      lagged_tmax
    ##    <chr>          <chr>      <date>     <dbl> <dbl> <dbl> <date>           <dbl>
    ##  1 CentralPark_NY USW000947… 2021-01-01   157   4.4   0.6 2021-01-01        NA  
    ##  2 CentralPark_NY USW000947… 2021-01-02    13  10.6   2.2 2021-01-01         4.4
    ##  3 CentralPark_NY USW000947… 2021-01-03    56   3.3   1.1 2021-01-01        10.6
    ##  4 CentralPark_NY USW000947… 2021-01-04     5   6.1   1.7 2021-01-01         3.3
    ##  5 CentralPark_NY USW000947… 2021-01-05     0   5.6   2.2 2021-01-01         6.1
    ##  6 CentralPark_NY USW000947… 2021-01-06     0   5     1.1 2021-01-01         5.6
    ##  7 CentralPark_NY USW000947… 2021-01-07     0   5    -1   2021-01-01         5  
    ##  8 CentralPark_NY USW000947… 2021-01-08     0   2.8  -2.7 2021-01-01         5  
    ##  9 CentralPark_NY USW000947… 2021-01-09     0   2.8  -4.3 2021-01-01         2.8
    ## 10 CentralPark_NY USW000947… 2021-01-10     0   5    -1.6 2021-01-01         2.8
    ## # ℹ 2,180 more rows

Use the variables you create

``` r
weather_df |> 
  group_by(name) |> 
  mutate(
    temp_change = tmax - lag(tmax)
  ) |> 
  summarise(
    sd_tmax_change = sd(temp_change, na.rm = TRUE),
    tmax_change_max = max(temp_change, na.rm = TRUE)
  )
```

    ## # A tibble: 3 × 3
    ##   name           sd_tmax_change tmax_change_max
    ##   <chr>                   <dbl>           <dbl>
    ## 1 CentralPark_NY           4.43            12.2
    ## 2 Molokai_HI               1.24             5.6
    ## 3 Waterhole_WA             3.04            11.1

Figure out what day it happened

``` r
weather_df |> 
  group_by(name) |> 
  mutate(
    temp_change = tmax - lag(tmax),
    change_rank = min_rank(desc(temp_change))
    )|> 
  filter(change_rank < 2)
```

    ## # A tibble: 4 × 9
    ## # Groups:   name [3]
    ##   name     id    date        prcp  tmax  tmin month      temp_change change_rank
    ##   <chr>    <chr> <date>     <dbl> <dbl> <dbl> <date>           <dbl>       <int>
    ## 1 Central… USW0… 2022-03-06    15  20     6.1 2022-03-01        12.2           1
    ## 2 Molokai… USW0… 2021-01-19     0  27.8  21.1 2021-01-01         5.6           1
    ## 3 Molokai… USW0… 2022-11-29     0  27.8  19.4 2022-11-01         5.6           1
    ## 4 Waterho… USS0… 2022-12-22    76   1.5 -17.2 2022-12-01        11.1           1

``` r
#Slightly slimmer version 
weather_df |> 
  group_by(name) |> 
  mutate(
    temp_change = tmax - lag(tmax)
    )|> 
  filter(min_rank(desc(temp_change)) < 2)
```

    ## # A tibble: 4 × 8
    ## # Groups:   name [3]
    ##   name           id          date        prcp  tmax  tmin month      temp_change
    ##   <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <date>           <dbl>
    ## 1 CentralPark_NY USW00094728 2022-03-06    15  20     6.1 2022-03-01        12.2
    ## 2 Molokai_HI     USW00022534 2021-01-19     0  27.8  21.1 2021-01-01         5.6
    ## 3 Molokai_HI     USW00022534 2022-11-29     0  27.8  19.4 2022-11-01         5.6
    ## 4 Waterhole_WA   USS0023B17S 2022-12-22    76   1.5 -17.2 2022-12-01        11.1

Let’s revist the pulse dataset

``` r
pulse_df =
  haven::read_sas("data/public_pulse_data.sas7bdat") |>  
  janitor::clean_names() |> 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  ) |> 
  mutate(visit = replace(visit, visit == "bl", "00m"),
         visit = fct_inorder(visit)
        )  


pulse_df |> 
  ggplot(aes(x = visit, y = bdi)) +
  geom_boxplot()
```

    ## Warning: Removed 879 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

<img src="eda_files/figure-gfm/unnamed-chunk-17-1.png" width="90%" />

``` r
pulse_df |> 
  group_by(visit) |> 
  summarise(
    mean_bdi = mean(bdi, na.rm =TRUE),
    median_bdi = median(bdi, na.rm = TRUE)
  ) |> 
  knitr::kable(digits=2)
```

| visit | mean_bdi | median_bdi |
|:------|---------:|-----------:|
| 00m   |     7.99 |          6 |
| 01m   |     6.05 |          4 |
| 06m   |     5.67 |          4 |
| 12m   |     6.10 |          4 |
