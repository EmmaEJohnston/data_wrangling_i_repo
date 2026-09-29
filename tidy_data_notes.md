Tidy Data Notes
================
Emma
2026-09-29

## Notes

``` r
library(tidyverse)
```

``` r
data("mtcars")
```

``` r
mtcars$mpg
```

    ##  [1] 21.0 21.0 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 17.8 16.4 17.3 15.2 10.4
    ## [16] 10.4 14.7 32.4 30.4 33.9 21.5 15.5 15.2 13.3 19.2 27.3 26.0 30.4 15.8 19.7
    ## [31] 15.0 21.4

``` r
mtcars$mpg = 1 
# this pulls something out and manipulates the whole data set. don't do this 
```

``` r
pull(mtcars, mpg) 
```

    ##  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

``` r
# if you have take something out, use pull function like this because it won't let you edit whole dataset
```

``` r
mtcars |> 
  select(mpg:disp) 
```

    ##                     mpg cyl  disp
    ## Mazda RX4             1   6 160.0
    ## Mazda RX4 Wag         1   6 160.0
    ## Datsun 710            1   4 108.0
    ## Hornet 4 Drive        1   6 258.0
    ## Hornet Sportabout     1   8 360.0
    ## Valiant               1   6 225.0
    ## Duster 360            1   8 360.0
    ## Merc 240D             1   4 146.7
    ## Merc 230              1   4 140.8
    ## Merc 280              1   6 167.6
    ## Merc 280C             1   6 167.6
    ## Merc 450SE            1   8 275.8
    ## Merc 450SL            1   8 275.8
    ## Merc 450SLC           1   8 275.8
    ## Cadillac Fleetwood    1   8 472.0
    ## Lincoln Continental   1   8 460.0
    ## Chrysler Imperial     1   8 440.0
    ## Fiat 128              1   4  78.7
    ## Honda Civic           1   4  75.7
    ## Toyota Corolla        1   4  71.1
    ## Toyota Corona         1   4 120.1
    ## Dodge Challenger      1   8 318.0
    ## AMC Javelin           1   8 304.0
    ## Camaro Z28            1   8 350.0
    ## Pontiac Firebird      1   8 400.0
    ## Fiat X1-9             1   4  79.0
    ## Porsche 914-2         1   4 120.3
    ## Lotus Europa          1   4  95.1
    ## Ford Pantera L        1   8 351.0
    ## Ferrari Dino          1   6 145.0
    ## Maserati Bora         1   8 301.0
    ## Volvo 142E            1   4 121.0

``` r
#be careful when loading multiple libraries. they could use the same words for different functions, and R will remember the most recently loaded library. to avoid this, add which package from the library you're using: 

mtcars |> 
  dplyr::select(mpg:disp)
```

    ##                     mpg cyl  disp
    ## Mazda RX4             1   6 160.0
    ## Mazda RX4 Wag         1   6 160.0
    ## Datsun 710            1   4 108.0
    ## Hornet 4 Drive        1   6 258.0
    ## Hornet Sportabout     1   8 360.0
    ## Valiant               1   6 225.0
    ## Duster 360            1   8 360.0
    ## Merc 240D             1   4 146.7
    ## Merc 230              1   4 140.8
    ## Merc 280              1   6 167.6
    ## Merc 280C             1   6 167.6
    ## Merc 450SE            1   8 275.8
    ## Merc 450SL            1   8 275.8
    ## Merc 450SLC           1   8 275.8
    ## Cadillac Fleetwood    1   8 472.0
    ## Lincoln Continental   1   8 460.0
    ## Chrysler Imperial     1   8 440.0
    ## Fiat 128              1   4  78.7
    ## Honda Civic           1   4  75.7
    ## Toyota Corolla        1   4  71.1
    ## Toyota Corona         1   4 120.1
    ## Dodge Challenger      1   8 318.0
    ## AMC Javelin           1   8 304.0
    ## Camaro Z28            1   8 350.0
    ## Pontiac Firebird      1   8 400.0
    ## Fiat X1-9             1   4  79.0
    ## Porsche 914-2         1   4 120.3
    ## Lotus Europa          1   4  95.1
    ## Ford Pantera L        1   8 351.0
    ## Ferrari Dino          1   6 145.0
    ## Maserati Bora         1   8 301.0
    ## Volvo 142E            1   4 121.0

# Tidy data

Rules for tidy data: \* data tables have an implied structure which the
“tidy data” framework makes explicit \*\* Columns are variables \*\*
Rows are observations \*\* every value has a cell

Why tidy your data? \* consistent data structures will simplify your
thought process \*\* especially true if you use tools designed to tidy
data \* data written for computers is easier to work with

Not all data are tidy: \* columns are values not variable names \*
single columns contain multiple variables \* data stored in multiple
tables WATCH OUT FOR THESE!

Relational data \* data spread across tables with defined relations \*
variables used to define these relations are *keys* \* tables are
combined by *joins*

Join types \* joining datasets x and y \*\* mostly use left joins and
full joins (both outer joins)

Key functions \* For tidying single tables \*\* pivot_longer –\> easier
to read as computer \*\* separate (separate column into two separate
columns)

- for untidying single tables: \*\* pivot_wider –\> easier to read as
  human

- For combining multiple tables \*\* bind_rows \*\* \*\_join –\>
  whichever join you’re doing
