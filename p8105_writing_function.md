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

    ##  [1]  0.96754250 -0.16009500  0.06307460 -0.33388965 -0.83540573  0.41092092
    ##  [7]  1.19430905 -0.91905444 -0.84355771  1.84476008  0.15166484 -0.44494105
    ## [13] -0.22242124 -2.23495129 -0.16153600  0.20727931 -0.70308859 -1.26129941
    ## [19] -0.23529491  1.66646877 -0.91428995 -0.09438963  2.27542855 -0.08698218
    ## [25]  0.44566558 -0.19762464 -0.50089706  0.17956187  1.71152073 -0.96847834

I want a function to compute z-scores

``` r
# naming the input x
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x_vec)
```

    ##  [1]  0.96754250 -0.16009500  0.06307460 -0.33388965 -0.83540573  0.41092092
    ##  [7]  1.19430905 -0.91905444 -0.84355771  1.84476008  0.15166484 -0.44494105
    ## [13] -0.22242124 -2.23495129 -0.16153600  0.20727931 -0.70308859 -1.26129941
    ## [19] -0.23529491  1.66646877 -0.91428995 -0.09438963  2.27542855 -0.08698218
    ## [25]  0.44566558 -0.19762464 -0.50089706  0.17956187  1.71152073 -0.96847834

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
    ## 1 0.0788  1.10

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
    ## 1  3.85  2.58

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
    ## 1  6.23  3.23

``` r
# the default mu and sigma will be overriden
sim_mean_sd(mu = 6, samp_size = 100, sigma = 3) #name matching
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.89  3.14

``` r
sim_mean_sd(samp_size = 100)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.15  3.95
