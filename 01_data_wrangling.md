Data Import
================

This file is for doing data import.

``` r
library(tidyverse)
```

How to specify where a file is: \* absolute path: start far away, line
by line, say where exactly the file is \* relative path: relative to
where you are right now, where is file \* always use relative paths

# Import our data

Import our first data set.

``` r
litters_df = 
  read_csv(file = "data_import_examples/FAS_litters.csv", skip = 0)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (4): Group, Litter Number, GD0 weight, GD18 weight
    ## dbl (4): GD of Birth, Pups born alive, Pups dead @ birth, Pups survive
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df = janitor::clean_names(litters_df)
```

Go back to look at OG data set, they used both NA and . to indicate
missing values. First, use janitor::clean_names(df) to clean up data
names. Janitor package - don’t need to load it all with library, you
just use this function so you can just load the clean names function.

Import your second dataset.

``` r
pups_df = 
  read_csv(file = "data_import_examples/FAS_pups.csv", skip = 3)
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Litter Number, PD ears
    ## dbl (4): Sex, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df = janitor::clean_names(pups_df) 
```

Can also skip writing file at beginning of parentheses (it expects
this). Skip importing first three lines of data from pups dataset,
because they’re blank.

Since you’re setting the janitor function equal to the df, you’re
permanently altering the file.

# Look at the data

``` r
litters_df
```

    ## # A tibble: 49 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2       27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  9 Con8  #2/95/3         <NA>       <NA>                 20               8
    ## 10 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
pups_df
```

    ## # A tibble: 313 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl> <chr>     <dbl>    <dbl>   <dbl>
    ##  1 #85               1 4            13        7      11
    ##  2 #85               1 4            13        7      12
    ##  3 #1/2/95/2         1 5            13        7       9
    ##  4 #1/2/95/2         1 5            13        8      10
    ##  5 #5/5/3/83/3-3     1 5            13        8      10
    ##  6 #5/5/3/83/3-3     1 5            14        6       9
    ##  7 #5/4/2/95/2       1 .            14        5       9
    ##  8 #4/2/95/3-3       1 4            13        6       8
    ##  9 #4/2/95/3-3       1 4            13        7       9
    ## 10 #2/2/95/3-2       1 4            NA        8      10
    ## # ℹ 303 more rows

Head or tail

``` r
head(litters_df, 20)
```

    ## # A tibble: 20 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>           <chr>      <chr>             <dbl>           <dbl>
    ##  1 Con7  #85             19.7       34.7                 20               3
    ##  2 Con7  #1/2/95/2       27         42                   19               8
    ##  3 Con7  #5/5/3/83/3-3   26         41.4                 19               6
    ##  4 Con7  #5/4/2/95/2     28.5       44.1                 19               5
    ##  5 Con7  #4/2/95/3-3     <NA>       <NA>                 20               6
    ##  6 Con7  #2/2/95/3-2     <NA>       <NA>                 20               6
    ##  7 Con7  #1/5/3/83/3-3/2 <NA>       <NA>                 20               9
    ##  8 Con8  #3/83/3-3       <NA>       <NA>                 20               9
    ##  9 Con8  #2/95/3         <NA>       <NA>                 20               8
    ## 10 Con8  #3/5/2/2/95     28.5       <NA>                 20               8
    ## 11 Con8  #5/4/3/83/3     28         <NA>                 19               9
    ## 12 Con8  #1/6/2/2/95-2   <NA>       <NA>                 20               7
    ## 13 Con8  #3/5/3/83/3-3-2 <NA>       <NA>                 20               8
    ## 14 Con8  #2/2/95/2       <NA>       <NA>                 19               5
    ## 15 Con8  #3/6/2/2/95-3   <NA>       <NA>                 20               7
    ## 16 Mod7  #59             17         33.4                 19               8
    ## 17 Mod7  #103            21.4       42.1                 19               9
    ## 18 Mod7  #1/82/3-2       <NA>       <NA>                 19               6
    ## 19 Mod7  #3/83/3-2       <NA>       <NA>                 19               8
    ## 20 Mod7  #2/95/2-2       <NA>       <NA>                 20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
tail(pups_df, 5)
```

    ## # A tibble: 5 × 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <dbl> <chr>     <dbl>    <dbl>   <dbl>
    ## 1 #2/95/2           2 3            13        6       8
    ## 2 #2/95/2           2 3            13        7       9
    ## 3 #82/4             2 4            13        7       9
    ## 4 #82/4             2 3            13        7       9
    ## 5 #82/4             2 3            13        7       9

Looking at specified number of rows at head or tail of dataset.

Skimming?

``` r
skimr::skim(pups_df)
```

|                                                  |         |
|:-------------------------------------------------|:--------|
| Name                                             | pups_df |
| Number of rows                                   | 313     |
| Number of columns                                | 6       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |         |
| Column type frequency:                           |         |
| character                                        | 2       |
| numeric                                          | 4       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |         |
| Group variables                                  | None    |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| litter_number |         0 |             1 |   3 |  15 |     0 |       49 |          0 |
| pd_ears       |         0 |             1 |   1 |   1 |     0 |        5 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |  mean |   sd |  p0 | p25 | p50 | p75 | p100 | hist  |
|:--------------|----------:|--------------:|------:|-----:|----:|----:|----:|----:|-----:|:------|
| sex           |         0 |          1.00 |  1.50 | 0.50 |   1 |   1 |   2 |   2 |    2 | ▇▁▁▁▇ |
| pd_eyes       |        13 |          0.96 | 12.99 | 0.62 |  12 |  13 |  13 |  13 |   15 | ▂▇▁▂▁ |
| pd_pivot      |        13 |          0.96 |  7.09 | 1.51 |   4 |   6 |   7 |   8 |   12 | ▂▇▂▂▁ |
| pd_walk       |         0 |          1.00 |  9.50 | 1.34 |   7 |   9 |   9 |  10 |   14 | ▆▇▇▂▁ |

This skims the file and give you an overview. Saying
`view(name of dataset)` in console will help you view dataset in R.
Don’t put in rmd because when you knit, you’ll get a pop up window.
