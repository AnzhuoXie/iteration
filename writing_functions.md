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

    ##  [1]  0.13422737 -0.68796681  1.12672469  0.24557991  1.86106185  0.12929645
    ##  [7]  1.28275502 -2.02258829  0.14631203  0.37349758 -1.43731256 -0.40563443
    ## [13]  0.49014285 -1.50925699 -0.01811023 -0.28326572  0.49922328  1.20303730
    ## [19]  0.43445182 -1.10540006 -0.07996241 -0.71737306  1.12426995  1.26356258
    ## [25]  0.06242370 -0.79857799 -0.34382784 -1.07840309 -1.45350880  1.56462191

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
