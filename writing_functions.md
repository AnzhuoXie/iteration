Writing Function
================

## Somethig somple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
```

I want a function to cpompute z-scores

``` r
z_score = function(x) {
  
  if(!is.numeric(x)) {
    stop('Input must be numeric')
  }
  
  if(length(x) < 3) {
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  return(z)
}

z_score(x_vec)
```

    ##  [1]  0.70312968 -0.68345423 -1.05769118  1.00236038  0.05677682 -1.32363894
    ##  [7]  1.28445080 -0.59616768  0.21241468 -1.09165237  2.84150419 -0.43234347
    ## [13]  0.29812259 -1.95684238  0.46021709 -0.26507029 -0.44467608 -0.57172703
    ## [19]  0.33590268  1.15461636 -0.35152596 -0.40409637  1.28977153 -0.21581527
    ## [25]  0.05591396 -1.11915615 -0.37104779  1.71262620 -0.37520770 -0.14769410

Try my function on some other things. These should give errors.

``` r
z_score(3)
```

    ## Error in z_score(3): Input must have at least three numbers

``` r
z_score("My name is Anderson")
```

    ## Error in z_score("My name is Anderson"): Input must be numeric

``` r
z_score(mtcars)
```

    ## Error in z_score(mtcars): Input must be numeric

``` r
z_score(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_score(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric

## Multiple outputs

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

Check the function works

``` r
mean_and_sd(x_vec)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.81  2.60

## Multiple inputs

``` r
sim_mean_sd = function(sample_size, mu = 0, sigma = 1) {  #mu=0, sigma=1 is the default value, it will be overwritten if you input the value you want
  
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

sim_mean_sd(sample_size = 100, mu = 4, sigma = 3)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.35  2.77

``` r
sim_mean_sd(100)
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.144 0.956
