tidyr tutorial
================
fg2399
2017/6/27

tidyr tutorial
==============

tidyr and dplyr provide fundamental functions for cleaning, processing, & manipulating data

gather
------

`gather` function will reshape wide format to long format

It is pretty similar to `melt` function in `reshape`

``` r
library(tidyr)
library(reshape2)
head(airquality)
```

    ##   Ozone Solar.R Wind Temp Month Day
    ## 1    41     190  7.4   67     5   1
    ## 2    36     118  8.0   72     5   2
    ## 3    12     149 12.6   74     5   3
    ## 4    18     313 11.5   62     5   4
    ## 5    NA      NA 14.3   56     5   5
    ## 6    28      NA 14.9   66     5   6

``` r
aql = gather(airquality, variable, value, -Month, -Day)
head(aql)
```

    ##   Month Day variable value
    ## 1     5   1    Ozone    41
    ## 2     5   2    Ozone    36
    ## 3     5   3    Ozone    12
    ## 4     5   4    Ozone    18
    ## 5     5   5    Ozone    NA
    ## 6     5   6    Ozone    28

``` r
aql2 = melt(airquality, id.vars = c("Month","Day"), variable.name = "variable", value.name = "value")
head(aql2)
```

    ##   Month Day variable value
    ## 1     5   1    Ozone    41
    ## 2     5   2    Ozone    36
    ## 3     5   3    Ozone    12
    ## 4     5   4    Ozone    18
    ## 5     5   5    Ozone    NA
    ## 6     5   6    Ozone    28

spread
------

`spread` function will reshape long format into wide format

It is pretty similar to `dcast` function in `reshape2`

``` r
aqw = spread(aql, key = "variable", value = "value")
head(aqw)
```

    ##   Month Day Ozone Solar.R Temp Wind
    ## 1     5   1    41     190   67  7.4
    ## 2     5   2    36     118   72  8.0
    ## 3     5   3    12     149   74 12.6
    ## 4     5   4    18     313   62 11.5
    ## 5     5   5    NA      NA   56 14.3
    ## 6     5   6    28      NA   66 14.9

``` r
aqw2 = dcast(aql, Month + Day ~ variable)
head(aqw2)
```

    ##   Month Day Ozone Solar.R Temp Wind
    ## 1     5   1    41     190   67  7.4
    ## 2     5   2    36     118   72  8.0
    ## 3     5   3    12     149   74 12.6
    ## 4     5   4    18     313   62 11.5
    ## 5     5   5    NA      NA   56 14.3
    ## 6     5   6    28      NA   66 14.9

unite
-----

`unite` merge two varibales into one. It combines two varibales of a single observation into one variable.

``` r
aql_united = unite(aql, col = date, Month, Day, sep = '.')
head(aql_united)
```

    ##   date variable value
    ## 1  5.1    Ozone    41
    ## 2  5.2    Ozone    36
    ## 3  5.3    Ozone    12
    ## 4  5.4    Ozone    18
    ## 5  5.5    Ozone    NA
    ## 6  5.6    Ozone    28

seperate
--------

`seperate` function will split a single variable into two. It is similar to split the key into two variables, using a regular expression to describe the character that seperates them.

``` r
aql_separate = separate(aql_united, col = date, into = c('Month','Day'), sep = '\\.')
head(aql_separate)
```

    ##   Month Day variable value
    ## 1     5   1    Ozone    41
    ## 2     5   2    Ozone    36
    ## 3     5   3    Ozone    12
    ## 4     5   4    Ozone    18
    ## 5     5   5    Ozone    NA
    ## 6     5   6    Ozone    28
