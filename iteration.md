Writing Functions
================
Yue Liu
2020-11-10

List columns are very useful imagine you have an input list in a data
frame You can map a function to each element of that input list, export
the output list, and save it in the same data frame

Imagine you have granular data nested within large units - make a list
storing your granular data table - add the granular data table list to a
data frame containing data on larger units Why stop? - you can store
more complex R objects, like output from regressions on each granular
data table in a list - you can add that list to your data frame

Keeping everything in one data frame with list columns means there are
fewer things to worry about

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = 0.2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 3.35331477 4.03729948 3.00054115 4.44891606 0.06845442 2.26323735
    ##  [7] 1.68584094 3.30393098 2.24061286 3.47645405 2.68017620 3.10157139
    ## [13] 1.97484831 3.13902589 3.65935941 2.66208031 1.77426255 1.05144889
    ## [19] 2.80032136 3.25381887
    ## 
    ## $b
    ##  [1]  -0.3942325   4.4438574  -1.1217883   1.5526493  -0.9755864   2.3253617
    ##  [7]   7.4095165  -1.0004248 -16.8787626  -4.1687290   7.8369663   3.9914272
    ## [13]   0.2604866   2.6763178  -8.9817187  -1.0448883   8.1046948  -7.5743774
    ## [19]  -4.7621847   5.9622773  -4.2845421   7.4519304   3.8327310   4.3816189
    ## [25]   5.7886847   9.9075887   0.5551177  -1.6055932   8.3748131   3.7458243
    ## 
    ## $c
    ##  [1]  9.830158 10.024266 10.315921  9.943447  9.878299 10.342892  9.957557
    ##  [8] 10.147378 10.190744  9.866433 10.065289 10.537310  9.941802  9.694786
    ## [15]  9.983951  9.841058 10.243255 10.162477 10.044094 10.011175  9.830805
    ## [22] 10.175522 10.047139 10.042515 10.006630 10.011418 10.047246  9.980150
    ## [29] 10.045467  9.876845 10.236578  9.694793 10.201847 10.071666 10.069612
    ## [36] 10.095857 10.234344  9.757265 10.077703 10.275289
    ## 
    ## $d
    ##  [1] -1.640471 -4.549065 -2.102200 -2.890937 -2.957619 -1.649575 -2.829887
    ##  [8] -2.146725 -2.962331 -2.004601 -3.708657 -4.725880 -1.942853 -3.533755
    ## [15] -2.931685 -3.446279 -1.747422 -2.666851 -2.447633 -3.873528

Pause and get my old function

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

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.70  1.04

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.19  5.93

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.179

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.84 0.916

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```
