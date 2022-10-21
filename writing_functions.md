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

    ##  [1] -1.78086336 -0.18822417  0.95185520  1.33072405 -0.60902794  2.36989954
    ##  [7] -1.66115549  0.30364540  1.67778347 -1.09840567  0.10267106  1.14664781
    ## [13] -0.59774192  1.05444919 -0.68041943  0.74523439 -1.38250258 -0.51214554
    ## [19] -0.52183385 -0.12451962 -0.85514356 -0.11373494  1.00413558 -0.93625023
    ## [25] -0.77327195  0.71798743  0.31449774  0.01855439  0.30844107 -0.21128610

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
    ## 1  4.01  2.99

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
    ## 1  4.34  3.05

``` r
sim_mean_sd(100)
```

    ## # A tibble: 1 × 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 0.00717  1.05

## Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page to review ….

Let’s turn that code into a function.

``` r
read_page_reviews = function(url) {
  
  html = read_html(url)

  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )

  reviews
  
}
```

let try the function.

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

all_reviews = 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
  )
```

## Mean scoping example

``` r
f = function(x){
  z = x + y
  z
}

x = 1
y = 2
f(x = y)
```

    ## [1] 4
