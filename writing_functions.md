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

    ##  [1] -0.5431468  0.2403932 -0.8107864  0.8501560  2.1263924 -0.6963249
    ##  [7]  0.3658143  1.4132624 -0.5831580 -0.8017394 -0.2746416  1.3363226
    ## [13]  0.4156142 -0.8654766  0.7103843  1.7719917 -0.7157621 -0.5316131
    ## [19] -0.4126068  0.5190422 -0.7090892 -0.8200365 -0.1329114  1.1892208
    ## [25] -0.9238503 -1.5626902 -1.4653630 -0.1546957  1.7249542 -0.6596560

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
    ## 1  4.28  2.91

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
    ## 1  4.55  2.79

``` r
sim_mean_sd(100)
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0654 0.966

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

## Function as arguments

``` r
my_summary = function(x, summ_func){
  
  summ_func(x)
  
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.501659

``` r
median(x_vec)
```

    ## [1] 3.389501

``` r
my_summary(x_vec, median)
```

    ## [1] 3.389501
