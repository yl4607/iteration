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
    ##  [1] 2.513049 3.749632 4.866917 3.835852 2.774242 4.200586 1.685256 4.221804
    ##  [9] 2.556372 3.249639 2.979463 3.235691 2.356392 2.888002 3.113868 3.228601
    ## [17] 3.505683 1.857518 2.936683 2.053025
    ## 
    ## $b
    ##  [1] -3.87611330 -8.53423439 -5.32445649  0.04588568 -2.29818133  0.09449782
    ##  [7]  1.05750367 -4.95527940  2.87289953  1.17759851 -2.44996546 -2.58184331
    ## [13]  4.36106525 -0.98099420 -7.80316344  0.32359519 -0.94034692  4.33867342
    ## [19] -5.31674221  5.15511015 -1.04454853  9.32945779 -3.12007230  3.44724928
    ## [25] -4.61995834  2.21679477 11.82916538  3.19586646  3.20594863 -3.97974018
    ## 
    ## $c
    ##  [1]  9.810773  9.710592  9.875461 10.296638  9.842130 10.133532 10.193561
    ##  [8]  9.943940  9.866704  9.870122 10.062555 10.086923  9.717967  9.828307
    ## [15]  9.756168 10.036503  9.996014 10.017188  9.839645  9.654081  9.826890
    ## [22] 10.362081 10.190544 10.071277 10.208776  9.918477  9.840573  9.894900
    ## [29]  9.888234  9.752862 10.006184  9.966616 10.346854  9.799197  9.927244
    ## [36] 10.128428 10.016761  9.916546  9.934518  9.776072
    ## 
    ## $d
    ##  [1] -3.463875 -1.449490 -2.950664 -2.715710 -3.384508 -2.029879 -3.856926
    ##  [8] -3.399624 -3.258207 -2.828824 -2.650903 -2.264920 -2.421257 -3.937914
    ## [15] -2.597606 -3.835265 -1.394320 -2.046959 -1.331879 -1.959588

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
    ## 1  3.09 0.821

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.172  4.68

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.176

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.69 0.825

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
