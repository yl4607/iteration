Writing Functions
================
Yue Liu
2020-11-10

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

# z score
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.33393664  0.08722491 -0.80151699 -0.89369834  0.03928410  0.58607238
    ##  [7]  0.49834436 -0.08720172 -0.48121643 -0.40938422 -0.47380652 -1.83030457
    ## [13] -0.41252880 -0.38369868  0.85655198  0.75541490  0.59938175  0.43373512
    ## [19]  0.17593351 -0.10092249  0.93511104 -1.16579112 -1.92340397  1.75159966
    ## [25]  0.10550745 -2.02304467  2.25711019  0.22111573  0.57881380  1.43925429

I want a function to compute z-scores

``` r
# naming the input x
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x_vec)
```

    ##  [1] -0.33393664  0.08722491 -0.80151699 -0.89369834  0.03928410  0.58607238
    ##  [7]  0.49834436 -0.08720172 -0.48121643 -0.40938422 -0.47380652 -1.83030457
    ## [13] -0.41252880 -0.38369868  0.85655198  0.75541490  0.59938175  0.43373512
    ## [19]  0.17593351 -0.10092249  0.93511104 -1.16579112 -1.92340397  1.75159966
    ## [25]  0.10550745 -2.02304467  2.25711019  0.22111573  0.57881380  1.43925429

Try my function on some othher
    things

``` r
z_scores(3) #NA
```

    ## [1] NA

``` r
z_scores("my name is jeff") #Error
```

    ## Error in x - mean(x): non-numeric argument to binary operator

``` r
z_scores(mtcars) #Error
```

    ## Error in is.data.frame(x): 'list' object cannot be coerced to type 'double'

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE)) # 0.5  0.5 -1.5  0.5
```

    ## [1]  0.5  0.5 -1.5  0.5

update the function

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3){
    stop("Input must have at least three numbers ")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}
```

``` r
z_scores(3) # Error in z_scores(3) : Input must have at least three numbers 
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is jeff") # Error in z_scores("my name is jeff") : Input must be numeric
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

## Multiple outputs

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3){
    stop("Input must have at least three numbers ")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean =  mean_x,
    sd = sd_x
  )

}
```

check if the function works

``` r
x_vec = rnorm(100) # mean = 0, sd = 1
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.101 0.963

## Multiple inputs

``` r
sim_data = 
  tibble(
    x = rnorm(100, mean = 4, sd = 3) # x columns
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.01  3.29

translate into a function

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4){
  
  sim_data = 
    tibble(
      x = rnorm(n = samp_size, mean = mu, sd = sigma) 
    )

  sim_data %>% 
    summarize(
      mean = mean(x),
      sd = sd(x)
    )
}

sim_mean_sd(100, 6, 3) #positional matching
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.89  2.97

``` r
# the default mu and sigma will be overriden
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3) #name matching
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.07  2.84

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.62  4.23

## Let’s review Napoleon Dynamite

``` r
nap_dyn_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

napoleon_html  = read_html(nap_dyn_url)

review_titles = 
  napoleon_html %>% 
  html_nodes(".a-text-bold span") %>% 
  html_text()

review_stars = 
  napoleon_html %>% 
  html_nodes("#cm_cr-review_list .review-rating") %>% 
  html_text() %>% 
  str_extract("^\\d") %>% 
  as.numeric()
  
review_text = 
  napoleon_html %>% 
  html_nodes(".review-text-content span") %>% 
  html_text()


review = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews…

Let’s turn that code into a function

``` r
read_page_reviews = function(url){
  html  = read_html(url)

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
    html_text()
  
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
  )
  
  reviews
}
```

Let’s try the
function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                               stars text                               
    ##    <chr>                               <dbl> <chr>                              
    ##  1 Boo                                     1 "\n  We rented this movie because …
    ##  2 Movie is still silly fun....amazon…     1 "\n  We are getting really frustra…
    ##  3 Brilliant and awkwardly funny.          5 "\n  I've watched this movie repea…
    ##  4 Great purchase price for great mov…     5 "\n  Great movie and real good dig…
    ##  5 Movie for memories                      5 "\n  I've been looking for this mo…
    ##  6 Love!                                   5 "\n  Love this movie. Great qualit…
    ##  7 Hilarious!                              5 "\n  Such a funny movie, definitel…
    ##  8 napoleon dynamite                       5 "\n  cool movie\n"                 
    ##  9 Top 5                                   5 "\n  Best MOVIE ever! Funny one li…
    ## 10 👍                                      5 "\n  Exactly as described and came…

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

dynamite_urls[1] #the first page
```

    ## [1] "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

``` r
all_reviews = 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[3]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
  )
```
