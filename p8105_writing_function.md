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

    ##  [1] -0.86154008  0.88320811  0.56802950 -1.37385204 -0.08290286 -0.21442900
    ##  [7] -1.43748655  1.34736977  0.90377086 -1.35709654  1.16902465 -1.22872710
    ## [13] -0.66575467  0.74231913  2.38554269  0.41531922  1.18216099 -0.72082671
    ## [19] -0.55197409 -0.45732054  0.68743191 -0.30173851  0.63200939 -0.25419352
    ## [25] -2.07868379 -0.73673625  0.23690387  0.58883269  0.13841501  0.44292447

I want a function to compute z-scores

``` r
# naming the input x
z_scores = function(x) {
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x_vec)
```

    ##  [1] -0.86154008  0.88320811  0.56802950 -1.37385204 -0.08290286 -0.21442900
    ##  [7] -1.43748655  1.34736977  0.90377086 -1.35709654  1.16902465 -1.22872710
    ## [13] -0.66575467  0.74231913  2.38554269  0.41531922  1.18216099 -0.72082671
    ## [19] -0.55197409 -0.45732054  0.68743191 -0.30173851  0.63200939 -0.25419352
    ## [25] -2.07868379 -0.73673625  0.23690387  0.58883269  0.13841501  0.44292447

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
