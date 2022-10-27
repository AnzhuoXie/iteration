Simulation
================

## Let’s simulate something

I have a function.

``` r
sim_mean_sd = function(sample_size, mu = 0, sigma = 1) { 
  
  sim_data = 
    tibble(
      x = rnorm(n = sample_size, mean = mu, sd = sigma)
    )
  
  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
}
```

I can “simulate” by this line.

``` r
sim_mean_sd(300)
```

    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0321 0.975

## Let’s simulate a lot.

let’s start with a for loop.

``` r
output = vector("list", 10)

for (i in 1:10) {
  
  output[[i]] = sim_mean_sd(100)
  
}

bind_rows(output)
```

    ## # A tibble: 10 × 2
    ##       mean    sd
    ##      <dbl> <dbl>
    ##  1  0.0365 1.01 
    ##  2  0.0309 0.952
    ##  3  0.0666 1.10 
    ##  4 -0.131  0.997
    ##  5  0.148  1.02 
    ##  6  0.0334 0.778
    ##  7 -0.0407 0.990
    ##  8  0.103  0.999
    ##  9 -0.0882 0.992
    ## 10  0.0561 1.02

Let’s use a loop function.

``` r
sim_results = 
  rerun(100, sim_mean_sd(30)) %>% 
  bind_rows
```

Let’s look at results…

``` r
sim_results %>% 
  ggplot(aes(x = mean)) + geom_density()
```

<img src="simualtion_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
sim_results %>% 
  summarize(
    avg_samp_mean = mean(mean),
    sd_samp_mean = sd(mean)
  )
```

    ## # A tibble: 1 × 2
    ##   avg_samp_mean sd_samp_mean
    ##           <dbl>        <dbl>
    ## 1        0.0329        0.182

``` r
sim_results %>% 
  ggplot(aes(x = sd)) + geom_density()
```

<img src="simualtion_files/figure-gfm/unnamed-chunk-5-2.png" width="90%" />
