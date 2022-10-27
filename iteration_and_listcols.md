Iteration and Listcols
================

## Lists

You can put everything in list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.99464 -0.71021  0.04332  0.06278  0.82856  2.81852

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]]
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]][1:3]
```

    ## [1] 5 6 7

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

# ‘for’ loop

Create a new list.

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)) {
    stop('Input must be numeric')
  }
  
  if(length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
tibble(
  mean = mean(x),
  sd = sd(x)
)
  
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.16  1.09

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.05  6.02

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.94 0.208

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.44 0.848

Let’s use a for loop:

``` r
output = vector('list', length = 4)

for (i in 1:4){
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map!

``` r
output = map(list_norm, mean_and_sd)
output = map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median)  ## spit out a different type rather than list, but dbl can only be used in the single value
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input") # this .id = "input" makes the id can also be spited out
```

## list columns!

``` r
listcol_df = 
  tibble(
    name = c('a','b','c','d'),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 3.453444 2.282347 3.106037 2.920659 4.643998 2.030277 2.774101 3.325024
    ##  [9] 3.578257 4.002871 3.874378 5.021803 1.856185 4.191718 1.718698 2.654772
    ## [17] 3.942774 1.328473 1.888079 4.691895
    ## 
    ## $b
    ##  [1]   8.0027556   5.4177823 -10.6924850  -0.2338734  -2.6005843  -0.6733953
    ##  [7]  -1.4050659   5.2723802   0.6752810  -3.6597125  -5.2772864   8.1518100
    ## [13]   0.6891296  -2.5231556 -10.7103102   3.1733668  -4.6854036   5.2260083
    ## [19] -10.7396783   4.3581522   5.2415226   4.0356228   1.1884405  10.4317817
    ## [25]   9.2343219   8.1341111  -5.9856031   7.4497782  -0.4995220   4.3936985
    ## 
    ## $c
    ##  [1] 10.073027  9.825541  9.963318  9.885045 10.187378 10.207753  9.631268
    ##  [8] 10.084883  9.796089  9.612831 10.087844 10.108866  9.813040  9.650560
    ## [15] 10.195498 10.007379 10.039643 10.041954 10.029979  9.657379  9.941543
    ## [22]  9.865660  9.680567  9.999454  9.654656  9.391450  9.989306  9.793723
    ## [29]  9.953650  9.946388  9.754983 10.040435 10.349178 10.264378  9.892656
    ## [36] 10.251543  9.931559  9.977937 10.149419 10.048423
    ## 
    ## $d
    ##  [1] -4.479680 -2.938588 -4.405243 -4.054732 -2.972662 -3.704791 -2.338774
    ##  [8] -3.709304 -3.419015 -2.733673 -2.491525 -4.474165 -4.291976 -2.749090
    ## [15] -2.892527 -3.053982 -3.653599 -1.779729 -5.027853 -3.578590

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.16  1.09

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.16  1.09
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.05  6.02
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.94 0.208
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.44 0.848

Can I just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.16  1.09
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.05  6.02
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.94 0.208
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.44 0.848

So, can I add a list column?

``` r
listcol_df = 
  listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    medians = map_dbl(samp, median)
  )
```

## Weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2022-09-23 21:49:27 (8.402)

    ## file min/max dates: 1869-01-01 / 2022-09-30

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-09-23 21:49:31 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-09-23 21:49:33 (0.95)

    ## file min/max dates: 1999-09-01 / 2022-09-30

Get our list columns…

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

``` r
weather_nest$data[[2]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows

Suppose I want to regress `tmax` and `tmin` for each station.

``` r
lm(tmax ~ tmin, data = weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

Let’s write a function.

``` r
weather_lm = function(df) {
  
  lm(tmin ~ tmax, data = df)
  
}

output = vector('list', 3)

for (i in 1:3) {
  
  output[[i]] = weather_lm(weather_nest$data[[i]])
  
}
```

What about a map in a list columns?

``` r
weather_nest = 
  weather_nest %>% 
  mutate(models = map(data, weather_lm))

weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmin ~ tmax, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmax  
    ##     -5.4581       0.8771  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmin ~ tmax, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmax  
    ##     -5.5811       0.9028  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmin ~ tmax, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmax  
    ##     -5.4173       0.7222
