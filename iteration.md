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
    ##  [1] 2.9680622 5.2842166 0.9268506 4.5117407 2.3205985 3.7114630 3.7729533
    ##  [8] 2.4215012 4.6056619 2.5832429 3.2818407 3.3062944 3.5656171 4.6676294
    ## [15] 3.2612691 1.9848988 2.5758979 4.4294180 2.7778939 2.0498619
    ## 
    ## $b
    ##  [1]  -1.73209392  -8.90331359   3.14713087  -7.94559854  -1.00291440
    ##  [6]   1.26537627 -12.20122303   1.56497392  -3.21275261  -2.87315014
    ## [11]  -2.79753727   1.96349073  -5.58783578   1.53029785  -1.38345881
    ## [16] -10.32850985   0.10817616 -11.26947388  -1.15703779   1.56062394
    ## [21]   2.55167269   7.78089426   4.66985872   3.21242150  -4.89606175
    ## [26]  -0.68537800   2.14515952   3.91433003  -0.06965928 -10.51923424
    ## 
    ## $c
    ##  [1] 10.471413 10.049577  9.818205  9.790620 10.015137 10.098080 10.235422
    ##  [8] 10.084993 10.302587  9.747783  9.969247  9.990677  9.694401  9.658614
    ## [15] 10.313389 10.096365  9.811549  9.939249 10.010073 10.239667  9.902548
    ## [22]  9.856042  9.740005  9.836427 10.010823 10.020370 10.026202  9.900620
    ## [29] 10.106251 10.058782  9.986784 10.186006 10.060347 10.083086  9.801706
    ## [36] 10.315418  9.990360 10.087967 10.045983 10.002819
    ## 
    ## $d
    ##  [1] -3.044706 -2.681989 -3.016689 -3.233420 -1.836775 -2.804405 -1.562658
    ##  [8] -3.745033 -4.274483 -1.837853 -2.718832 -3.684625 -2.444805 -1.789202
    ## [15] -2.781363 -4.022637 -2.682578 -3.405554 -3.320012 -3.818279

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
    ## 1  3.25  1.09

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.71  5.18

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.182

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.94 0.778

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map\!

``` r
map_output = map(list_norm, mean_and_sd)
# similar as the for loop output
```

What if you want a different function..?

``` r
output = map(list_norm, IQR) # can be a standard function as well
```

``` r
output = map_dbl(list_norm, median) # give a vector
output = map_dbl(list_norm, median, .id = "input") 
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input") 
# give a dataframe
# kepe tracking the input names as well
```

## List columns

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
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
    ##  [1] 2.9680622 5.2842166 0.9268506 4.5117407 2.3205985 3.7114630 3.7729533
    ##  [8] 2.4215012 4.6056619 2.5832429 3.2818407 3.3062944 3.5656171 4.6676294
    ## [15] 3.2612691 1.9848988 2.5758979 4.4294180 2.7778939 2.0498619
    ## 
    ## $b
    ##  [1]  -1.73209392  -8.90331359   3.14713087  -7.94559854  -1.00291440
    ##  [6]   1.26537627 -12.20122303   1.56497392  -3.21275261  -2.87315014
    ## [11]  -2.79753727   1.96349073  -5.58783578   1.53029785  -1.38345881
    ## [16] -10.32850985   0.10817616 -11.26947388  -1.15703779   1.56062394
    ## [21]   2.55167269   7.78089426   4.66985872   3.21242150  -4.89606175
    ## [26]  -0.68537800   2.14515952   3.91433003  -0.06965928 -10.51923424
    ## 
    ## $c
    ##  [1] 10.471413 10.049577  9.818205  9.790620 10.015137 10.098080 10.235422
    ##  [8] 10.084993 10.302587  9.747783  9.969247  9.990677  9.694401  9.658614
    ## [15] 10.313389 10.096365  9.811549  9.939249 10.010073 10.239667  9.902548
    ## [22]  9.856042  9.740005  9.836427 10.010823 10.020370 10.026202  9.900620
    ## [29] 10.106251 10.058782  9.986784 10.186006 10.060347 10.083086  9.801706
    ## [36] 10.315418  9.990360 10.087967 10.045983 10.002819
    ## 
    ## $d
    ##  [1] -3.044706 -2.681989 -3.016689 -3.233420 -1.836775 -2.804405 -1.562658
    ##  [8] -3.745033 -4.274483 -1.837853 -2.718832 -3.684625 -2.444805 -1.789202
    ## [15] -2.781363 -4.022637 -2.682578 -3.405554 -3.320012 -3.818279

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.25  1.09

``` r
#listcol_df: tibble
#samp column: list
#[[1]] the first element of that list
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.71  5.18

Can I just…map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.25  1.09
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -1.71  5.18
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.182
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.94 0.778

so … can I add a list column?

``` r
listcol_df = 
  listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    medians = map_dbl(samp, median))
```
