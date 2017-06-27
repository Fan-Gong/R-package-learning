reshape tutorial
================
fg2399
2017/6/23

Reshape2 Tutoria
================

reshape2 is an R package that makes it easy to transform data between wide and long formats.

wide data has a column for each variable. what we see normally are usually wide data.

long-format data has a column for possible variable types and a column for the values of those variables. In reality, we need long-format data much commonly than wide-format data. For example, `ggplot2` requires long-format data(technically tidy data), `plyr` requires long-format data.

`reshape2` is based around two key functions: melt and cast.

melt
----

melt takes wide-format data and melt it into long-format data.

``` r
library(reshape2)
names(airquality) = tolower(names(airquality))
head(airquality)
```

    ##   ozone solar.r wind temp month day
    ## 1    41     190  7.4   67     5   1
    ## 2    36     118  8.0   72     5   2
    ## 3    12     149 12.6   74     5   3
    ## 4    18     313 11.5   62     5   4
    ## 5    NA      NA 14.3   56     5   5
    ## 6    28      NA 14.9   66     5   6

``` r
#default melt
aql = melt(airquality, variable.name = "climate_variable", value.name = "climate_value")
```

    ## No id variables; using all as measure variables

``` r
head(aql)
```

    ##   climate_variable climate_value
    ## 1            ozone            41
    ## 2            ozone            36
    ## 3            ozone            12
    ## 4            ozone            18
    ## 5            ozone            NA
    ## 6            ozone            28

``` r
tail(aql)
```

    ##     climate_variable climate_value
    ## 913              day            25
    ## 914              day            26
    ## 915              day            27
    ## 916              day            28
    ## 917              day            29
    ## 918              day            30

By default, `melt` has assumed that all columns with numeric values are variables with values. We can also add an ID variable that identify individual rows of data.

``` r
#default + ID variable
aql = melt(airquality, id.vars = c("month", "day"),variable.name = "climate_variable", value.name = "climate_value")
head(aql)
```

    ##   month day climate_variable climate_value
    ## 1     5   1            ozone            41
    ## 2     5   2            ozone            36
    ## 3     5   3            ozone            12
    ## 4     5   4            ozone            18
    ## 5     5   5            ozone            NA
    ## 6     5   6            ozone            28

cast
----

In `reshape2` there are multiple `cast` functions. Since you will most commonly work with `data.frame` objects, we'll explore the `dcast` function.

Here, we need to tell `dcast` that `month` and `day` are the ID variables(we want a column for each) and that `variable` describes the measured variables. Since there is only one remaining column, `dcast` will figure out that it contains the values themselves. We could explicitly declare this with `value.var`

``` r
aqw = dcast(aql, month + day ~ climate_variable)
```

    ## Using climate_value as value column: use value.var to override.

``` r
head(aqw)
```

    ##   month day ozone solar.r wind temp
    ## 1     5   1    41     190  7.4   67
    ## 2     5   2    36     118  8.0   72
    ## 3     5   3    12     149 12.6   74
    ## 4     5   4    18     313 11.5   62
    ## 5     5   5    NA      NA 14.3   56
    ## 6     5   6    28      NA 14.9   66

``` r
#you can also do some aggregation by using dcast

aqw = dcast(aql, month ~ climate_variable, fun.aggregate = mean)
```

    ## Using climate_value as value column: use value.var to override.

``` r
head(aqw)
```

    ##   month ozone  solar.r      wind     temp
    ## 1     5    NA       NA 11.622581 65.54839
    ## 2     6    NA 190.1667 10.266667 79.10000
    ## 3     7    NA 216.4839  8.941935 83.90323
    ## 4     8    NA       NA  8.793548 83.96774
    ## 5     9    NA 167.4333 10.180000 76.90000
