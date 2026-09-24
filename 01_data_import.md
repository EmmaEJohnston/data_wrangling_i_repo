Data Import
================
2026-09-22

This file is for doing data import.

``` r
library(tidyverse)
library(readxl)
library(haven)
```

How to specify where a file is: \* absolute path: start far away, line
by line, say where exactly the file is \* relative path: relative to
where you are right now, where is file \* always use relative paths

# Import our data

Import our first data set.

``` r
litters_df = 
  read_csv(file = "data_import_examples/FAS_litters.csv", 
           na = c("", ".", "NA")
           )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
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
  read_csv(file = "data_import_examples/FAS_pups.csv", 
           skip = 3, 
           na = c("", ".", "NA")
        )
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
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
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## # ℹ 39 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
pups_df
```

    ## # A tibble: 313 × 6
    ##    litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##    <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ##  1 #85               1       4      13        7      11
    ##  2 #85               1       4      13        7      12
    ##  3 #1/2/95/2         1       5      13        7       9
    ##  4 #1/2/95/2         1       5      13        8      10
    ##  5 #5/5/3/83/3-3     1       5      13        8      10
    ##  6 #5/5/3/83/3-3     1       5      14        6       9
    ##  7 #5/4/2/95/2       1      NA      14        5       9
    ##  8 #4/2/95/3-3       1       4      13        6       8
    ##  9 #4/2/95/3-3       1       4      13        7       9
    ## 10 #2/2/95/3-2       1       4      NA        8      10
    ## # ℹ 303 more rows

Head or tail

``` r
head(litters_df, 20)
```

    ## # A tibble: 20 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2             27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #3/83/3-3             NA          NA            20               9
    ##  9 Con8  #2/95/3               NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95           28.5        NA            20               8
    ## 11 Con8  #5/4/3/83/3           28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-3-2       NA          NA            20               8
    ## 14 Con8  #2/2/95/2             NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## 16 Mod7  #59                   17          33.4          19               8
    ## 17 Mod7  #103                  21.4        42.1          19               9
    ## 18 Mod7  #1/82/3-2             NA          NA            19               6
    ## 19 Mod7  #3/83/3-2             NA          NA            19               8
    ## 20 Mod7  #2/95/2-2             NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
tail(pups_df, 5)
```

    ## # A tibble: 5 × 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ## 1 #2/95/2           2       3      13        6       8
    ## 2 #2/95/2           2       3      13        7       9
    ## 3 #82/4             2       4      13        7       9
    ## 4 #82/4             2       3      13        7       9
    ## 5 #82/4             2       3      13        7       9

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
| character                                        | 1       |
| numeric                                          | 5       |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |         |
| Group variables                                  | None    |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| litter_number |         0 |             1 |   3 |  15 |     0 |       49 |          0 |

**Variable type: numeric**

| skim_variable | n_missing | complete_rate |  mean |   sd |  p0 | p25 | p50 | p75 | p100 | hist  |
|:--------------|----------:|--------------:|------:|-----:|----:|----:|----:|----:|-----:|:------|
| sex           |         0 |          1.00 |  1.50 | 0.50 |   1 |   1 |   2 |   2 |    2 | ▇▁▁▁▇ |
| pd_ears       |        18 |          0.94 |  3.68 | 0.59 |   2 |   3 |   4 |   4 |    5 | ▁▅▁▇▁ |
| pd_eyes       |        13 |          0.96 | 12.99 | 0.62 |  12 |  13 |  13 |  13 |   15 | ▂▇▁▂▁ |
| pd_pivot      |        13 |          0.96 |  7.09 | 1.51 |   4 |   6 |   7 |   8 |   12 | ▂▇▂▂▁ |
| pd_walk       |         0 |          1.00 |  9.50 | 1.34 |   7 |   9 |   9 |  10 |   14 | ▆▇▇▂▁ |

This skims the file and give you an overview. Saying
`view(name of dataset)` in console will help you view dataset in R.
Don’t put in rmd because when you knit, you’ll get a pop up window.

Don’t use read.csv, only use read_csv (the new and improved version)

look at ?read_csv, you’ll see what you can do within the function,
including na = c(““,”NA”), (that excludes the data that is blank or says
NA)

# Fixing the missing data in dataframes

Just go back to OG load and add line of `na = c("", ".", "NA")` and then
re-run. This tells R to ignore those values (treat them as a missing
value), rather than making the variable a character value.

# Oh excel… Importing from Excel

Go back to top, and load the readxl library. Jenny Bryan made `readxl`
solve our problems.

``` r
mlb_df = 
  read_excel("data_import_examples/mlb11.xlsx")
```

Look at the data

``` r
mlb_df
```

    ## # A tibble: 30 × 12
    ##    team        runs at_bats  hits homeruns bat_avg strikeouts stolen_bases  wins
    ##    <chr>      <dbl>   <dbl> <dbl>    <dbl>   <dbl>      <dbl>        <dbl> <dbl>
    ##  1 Texas Ran…   855    5659  1599      210   0.283        930          143    96
    ##  2 Boston Re…   875    5710  1600      203   0.28        1108          102    90
    ##  3 Detroit T…   787    5563  1540      169   0.277       1143           49    95
    ##  4 Kansas Ci…   730    5672  1560      129   0.275       1006          153    71
    ##  5 St. Louis…   762    5532  1513      162   0.273        978           57    90
    ##  6 New York …   718    5600  1477      108   0.264       1085          130    77
    ##  7 New York …   867    5518  1452      222   0.263       1138          147    97
    ##  8 Milwaukee…   721    5447  1422      185   0.261       1083           94    96
    ##  9 Colorado …   735    5544  1429      163   0.258       1201          118    73
    ## 10 Houston A…   615    5598  1442       95   0.258       1164          118    56
    ## # ℹ 20 more rows
    ## # ℹ 3 more variables: new_onbase <dbl>, new_slug <dbl>, new_obs <dbl>

Can use ?read_excel to specify stuff. Can specify which sheet, which
cell range.

Load the LotR data.

Import FOTR words.

``` r
fotr_df =
  read_excel("data_import_examples/LotR_Words.xlsx",
             range = "B3:D6"
             )

fotr_df
```

    ## # A tibble: 3 × 3
    ##   Race   Female  Male
    ##   <chr>   <dbl> <dbl>
    ## 1 Elf      1229   971
    ## 2 Hobbit     14  3644
    ## 3 Man         0  1995

Range says which cells to include. Don’t include title. You can include
sheet and range togeter (see help file)

Same for Two Towers:

``` r
tt_df =
  read_excel("data_import_examples/LotR_Words.xlsx",
             range = "F3:H6"
             )

tt_df
```

    ## # A tibble: 3 × 3
    ##   Race   Female  Male
    ##   <chr>   <dbl> <dbl>
    ## 1 Elf       331   513
    ## 2 Hobbit      0  2463
    ## 3 Man       401  3589

If you open a microsoft doc (Excel or word) while in repo, it will ask
you to commit. Don’t! Just close the file and it will go away.

# Import SAS files

Use the haven library

Read in the PULSE dataset.

``` r
pulse_df =
  read_sas("data_import_examples/public_pulse_data.sas7bdat")

pulse_df = janitor::clean_names(pulse_df)
```

What is so bad about ‘read.csv()’?? Import pups with ‘read.csv()’ DON’T
use. Lazy about variable names, super slow for big data sets. Also
veryyy long. shows you everything.

``` r
pups_df_base = 
  read.csv("data_import_examples/FAS_pups.csv")

pups_df_base
```

    ##                   X X1...male..2...female     X.1     X.2      X.3     X.4
    ## 1                                                                         
    ## 2                                                                         
    ## 3     Litter Number                   Sex PD ears PD eyes PD pivot PD walk
    ## 4               #85                     1       4      13        7      11
    ## 5               #85                     1       4      13        7      12
    ## 6         #1/2/95/2                     1       5      13        7       9
    ## 7         #1/2/95/2                     1       5      13        8      10
    ## 8     #5/5/3/83/3-3                     1       5      13        8      10
    ## 9     #5/5/3/83/3-3                     1       5      14        6       9
    ## 10      #5/4/2/95/2                     1       .      14        5       9
    ## 11      #4/2/95/3-3                     1       4      13        6       8
    ## 12      #4/2/95/3-3                     1       4      13        7       9
    ## 13      #2/2/95/3-2                     1       4    <NA>        8      10
    ## 14  #1/5/3/83/3-3/2                     1       4    <NA>     <NA>       9
    ## 15  #1/5/3/83/3-3/2                     1       4    <NA>        7       9
    ## 16  #1/5/3/83/3-3/2                     1       4    <NA>        7       9
    ## 17  #1/5/3/83/3-3/2                     1       4    <NA>        7       9
    ## 18  #1/5/3/83/3-3/2                     1       4    <NA>        7       9
    ## 19              #85                     2       4      13        6      11
    ## 20        #1/2/95/2                     2       4      13        7       9
    ## 21        #1/2/95/2                     2       4      13        7      10
    ## 22        #1/2/95/2                     2       5      13        8      10
    ## 23        #1/2/95/2                     2       5      13        8      10
    ## 24        #1/2/95/2                     2       5      13        6      10
    ## 25    #5/5/3/83/3-3                     2       5      13        8      10
    ## 26    #5/5/3/83/3-3                     2       5      14        7      10
    ## 27    #5/5/3/83/3-3                     2       5      14        8      10
    ## 28      #5/4/2/95/2                     2       .      14        7      10
    ## 29      #5/4/2/95/2                     2       .      14        7      10
    ## 30      #5/4/2/95/2                     2       .      14        7      10
    ## 31      #4/2/95/3-3                     2       4      13        5       7
    ## 32      #4/2/95/3-3                     2       4      13        7       9
    ## 33      #4/2/95/3-3                     2       4      13        6       8
    ## 34      #4/2/95/3-3                     2       4      13        7       9
    ## 35      #2/2/95/3-2                     2       4    <NA>        7      10
    ## 36      #2/2/95/3-2                     2       4    <NA>        8      10
    ## 37      #2/2/95/3-2                     2       4    <NA>        8      11
    ## 38  #1/5/3/83/3-3/2                     2       4    <NA>        7       9
    ## 39  #1/5/3/83/3-3/2                     2       4    <NA>        7       9
    ## 40  #1/5/3/83/3-3/2                     2       4    <NA>        7       9
    ## 41  #1/5/3/83/3-3/2                     2       4    <NA>        7       9
    ## 42        #3/83/3-3                     1       3      13        4       7
    ## 43        #3/83/3-3                     1       3      13     <NA>       7
    ## 44        #3/83/3-3                     1       3      13        5       7
    ## 45        #3/83/3-3                     1       3      12        5       8
    ## 46        #3/83/3-3                     1       4      13        7       9
    ## 47          #2/95/3                     1       4      13        6       9
    ## 48          #2/95/3                     1       3      13        4       8
    ## 49          #2/95/3                     1       3      13        4       8
    ## 50          #2/95/3                     1       3      12        6       8
    ## 51      #3/5/2/2/95                     1       4      13        6       8
    ## 52      #3/5/2/2/95                     1       4      13        5       8
    ## 53      #3/5/2/2/95                     1       4      13        5       8
    ## 54      #3/5/2/2/95                     1       4      13        5       8
    ## 55      #5/4/3/83/3                     1       4      13        7      10
    ## 56      #5/4/3/83/3                     1       4      14        7       9
    ## 57      #5/4/3/83/3                     1       4      14        6       9
    ## 58      #5/4/3/83/3                     1       4      14        6      10
    ## 59      #5/4/3/83/3                     1       4      14        7       9
    ## 60    #1/6/2/2/95-2                     1       3      13        7       9
    ## 61    #1/6/2/2/95-2                     1       4      13        7       9
    ## 62  #3/5/3/83/3-3-2                     1       4      13        8      10
    ## 63  #3/5/3/83/3-3-2                     1       4      13        7       9
    ## 64  #3/5/3/83/3-3-2                     1       4      13        8      10
    ## 65  #3/5/3/83/3-3-2                     1       4      13        8      10
    ## 66        #2/2/95/2                     1       5      14        7       9
    ## 67        #2/2/95/2                     1       4      14        8      11
    ## 68    #3/6/2/2/95-3                     1       3      13        7       9
    ## 69    #3/6/2/2/95-3                     1       3      13        7       9
    ## 70    #3/6/2/2/95-3                     1       3      13        6       8
    ## 71    #3/6/2/2/95-3                     1       3      12        6       8
    ## 72    #3/6/2/2/95-3                     1       3      14        6       8
    ## 73        #3/83/3-3                     2       3      13     <NA>       8
    ## 74        #3/83/3-3                     2       3      13        4       7
    ## 75        #3/83/3-3                     2       3      12        6       8
    ## 76          #2/95/3                     2       3      12        6       9
    ## 77          #2/95/3                     2       3      13        6       8
    ## 78          #2/95/3                     2       4      12        6       9
    ## 79          #2/95/3                     2       3      13        6       8
    ## 80      #3/5/2/2/95                     2       4      13        6       9
    ## 81      #3/5/2/2/95                     2       4      12        5       7
    ## 82      #3/5/2/2/95                     2       3      13        5       9
    ## 83      #3/5/2/2/95                     2       4      13        5       9
    ## 84      #5/4/3/83/3                     2       4      13        7       9
    ## 85      #5/4/3/83/3                     2       4      13        7      10
    ## 86      #5/4/3/83/3                     2       4      13        7       9
    ## 87    #1/6/2/2/95-2                     2       3      13        7       9
    ## 88    #1/6/2/2/95-2                     2       3      13        5       8
    ## 89    #1/6/2/2/95-2                     2       4      13        7       9
    ## 90    #1/6/2/2/95-2                     2       4      13        8      10
    ## 91  #3/5/3/83/3-3-2                     2       4      13        4       9
    ## 92  #3/5/3/83/3-3-2                     2       4      13        7       9
    ## 93  #3/5/3/83/3-3-2                     2       4      13        7      10
    ## 94  #3/5/3/83/3-3-2                     2       4      13        7       9
    ## 95        #2/2/95/2                     2       4      13        6       8
    ## 96        #2/2/95/2                     2       4      13        9      11
    ## 97    #3/6/2/2/95-3                     2       3      12        6       8
    ## 98    #3/6/2/2/95-3                     2       3      12        7       9
    ## 99            #84/2                     1       3      13        5       8
    ## 100           #84/2                     1       3      13        7      10
    ## 101           #84/2                     1       3      13        4       7
    ## 102            #107                     1       4      13        9      11
    ## 103            #107                     1       4      13        9      11
    ## 104            #107                     1       4      13       10      12
    ## 105            #107                     1       4      13        9      11
    ## 106            #107                     1       4      13        9      11
    ## 107            #107                     1       4      13        9      11
    ## 108            #107                     1       4      13       10      12
    ## 109           #85/2                     1       4      13        9      11
    ## 110           #85/2                     1       4      13       10      12
    ## 111             #98                     1       3      13        7      10
    ## 112             #98                     1       4      13        9      11
    ## 113             #98                     1       4      13        9      11
    ## 114             #98                     1       4      13     <NA>      10
    ## 115             #98                     1       3      13        9      11
    ## 116            #102                     1       4      13        7      11
    ## 117            #102                     1       4      13        9      11
    ## 118            #101                     1       3      12       10      12
    ## 119            #101                     1       3      13        7       9
    ## 120            #101                     1       4      12        6       8
    ## 121            #101                     1       4      12        6      11
    ## 122            #111                     1       4      13        5      10
    ## 123           #84/2                     2       3      13        5      12
    ## 124           #84/2                     2       3      13        6       8
    ## 125           #84/2                     2       3      12        8      10
    ## 126           #84/2                     2       3      13        5       8
    ## 127           #84/2                     2       3      13        9      11
    ## 128            #107                     2       4      13        8      10
    ## 129           #85/2                     2       4      12        9      11
    ## 130           #85/2                     2       4      13       10      12
    ## 131           #85/2                     2       4      13        9      11
    ## 132           #85/2                     2       4      13       10      12
    ## 133             #98                     2       2      13        7      10
    ## 134             #98                     2       4      13        9      11
    ## 135             #98                     2       4      13        7      10
    ## 136             #98                     2       3      13        9      11
    ## 137            #102                     2       4      14        9      11
    ## 138            #102                     2       3      13        8      11
    ## 139            #102                     2       3      13        9      11
    ## 140            #102                     2       4      13        8      10
    ## 141            #102                     2       3      13        9      11
    ## 142            #101                     2       3      12        9      12
    ## 143            #101                     2       3      14        9      11
    ## 144            #101                     2       3      12        6      10
    ## 145            #101                     2       4      12        9      11
    ## 146            #101                     2       4      12        8      11
    ## 147            #111                     2       4      13        5      10
    ## 148            #111                     2       4      13        5      10
    ## 149             #59                     1       4      14       10      12
    ## 150             #59                     1       4      14        8      11
    ## 151             #59                     1       4      13       12      12
    ## 152            #103                     1       4      13        8      10
    ## 153            #103                     1       4      14        7       9
    ## 154            #103                     1       3      13        8      10
    ## 155       #1/82/3-2                     1       4      13        5      10
    ## 156       #1/82/3-2                     1       4      13     <NA>       8
    ## 157       #3/83/3-2                     1       4      13        6       9
    ## 158       #3/83/3-2                     1       4      13        6       9
    ## 159       #3/83/3-2                     1       4      13        6       8
    ## 160       #3/83/3-2                     1       4      13        6       8
    ## 161       #3/83/3-2                     1       4      13        6       8
    ## 162       #3/83/3-2                     1       4      12        6       9
    ## 163       #2/95/2-2                     1       4      13        6       8
    ## 164       #2/95/2-2                     1       4      13        6       8
    ## 165       #2/95/2-2                     1       4      13        6       8
    ## 166       #2/95/2-2                     1       4      13        7       9
    ## 167       #3/82/3-2                     1       3      13        6       8
    ## 168       #3/82/3-2                     1       4      13        6       8
    ## 169       #4/2/95/2                     1       4      14        8      10
    ## 170       #4/2/95/2                     1       4      14     <NA>      11
    ## 171       #4/2/95/2                     1       4      14     <NA>      11
    ## 172       #4/2/95/2                     1       4      14        7       9
    ## 173     #5/3/83/5-2                     1       4      13        6      10
    ## 174     #5/3/83/5-2                     1       4      14     <NA>      10
    ## 175     #5/3/83/5-2                     1       4      14        4      10
    ## 176      #8/110/3-2                     1       3      13        6       8
    ## 177      #8/110/3-2                     1       3      13        6       8
    ## 178      #8/110/3-2                     1       3      13        6       8
    ## 179      #8/110/3-2                     1       4      13        6       8
    ## 180            #106                     1       3      13       10      12
    ## 181           #94/2                     1       .      13     <NA>       9
    ## 182             #62                     1       5      14       11      13
    ## 183             #62                     1       5      15       10      12
    ## 184             #62                     1       5      15       11      13
    ## 185             #59                     2       4      13       10      12
    ## 186             #59                     2       4      13        8      10
    ## 187            #103                     2       3      13        7       9
    ## 188            #103                     2       3      12        7       9
    ## 189            #103                     2       3      13        7       9
    ## 190            #103                     2       3      12        7       9
    ## 191            #103                     2       3      13        8      10
    ## 192            #103                     2       4      13        6       9
    ## 193       #1/82/3-2                     2       4      13        8      10
    ## 194       #1/82/3-2                     2       4      13        5      10
    ## 195       #1/82/3-2                     2       5      13        6      10
    ## 196       #1/82/3-2                     2       4      13        8      10
    ## 197       #3/83/3-2                     2       4      12     <NA>       8
    ## 198       #3/83/3-2                     2       4      12        6       8
    ## 199       #2/95/2-2                     2       4      13        6       9
    ## 200       #2/95/2-2                     2       4      13        6       8
    ## 201       #2/95/2-2                     2       4      13        5       8
    ## 202       #3/82/3-2                     2       4      13        5       8
    ## 203       #3/82/3-2                     2       3      12        4       8
    ## 204       #3/82/3-2                     2       3      12        6       8
    ## 205       #4/2/95/2                     2       4      14        8      11
    ## 206       #4/2/95/2                     2       4      14        7       9
    ## 207       #4/2/95/2                     2       4      14        7      10
    ## 208     #5/3/83/3-2                     2       3      12     <NA>       8
    ## 209     #5/3/83/3-2                     2       3      13     <NA>      10
    ## 210      #8/110/3-2                     2       3      12        4       9
    ## 211      #8/110/3-2                     2       3      13        6       8
    ## 212      #8/110/3-2                     2       4      14        6       9
    ## 213      #8/110/3-2                     2       4      13        6       8
    ## 214      #8/110/3-2                     2       4      13        6       8
    ## 215            #106                     2       3      14        8      10
    ## 216           #94/2                     2       .      14       11      13
    ## 217           #94/2                     2       .      13     <NA>       9
    ## 218             #62                     2       5      13       10      12
    ## 219             #53                     1       4      13       10      12
    ## 220             #53                     1       3      13        9      12
    ## 221             #53                     1       4      13        8      12
    ## 222             #53                     1       3      13       10      12
    ## 223             #53                     1       4      13        9      11
    ## 224             #79                     1       4      14        9      11
    ## 225             #79                     1       4      14       12      14
    ## 226             #79                     1       4      14        8      10
    ## 227             #79                     1       4      14        6       9
    ## 228             #79                     1       4      14       10      13
    ## 229            #100                     1       3      13        7       9
    ## 230            #100                     1       3      13        8      10
    ## 231           #4/84                     1       3      13        7       9
    ## 232           #4/84                     1       3      13        6      10
    ## 233           #4/84                     1       4      13        7      10
    ## 234            #108                     1       3      13        5       7
    ## 235            #108                     1       3      12        6       8
    ## 236            #108                     1       3      13        6       8
    ## 237            #108                     1       3      13        6       8
    ## 238             #99                     1       .      12        8      10
    ## 239             #99                     1       .      13        7       9
    ## 240             #99                     1       .      13        5       9
    ## 241             #99                     1       .      12        6       9
    ## 242            #110                     1       .      12        6       8
    ## 243             #53                     2       3      13       11      13
    ## 244             #53                     2       4      13       10      12
    ## 245             #79                     2       4      13        9      11
    ## 246             #79                     2       4      14       12      14
    ## 247            #100                     2       4      13        9      11
    ## 248            #100                     2       3      13        9      11
    ## 249            #100                     2       3      12        9      11
    ## 250            #100                     2       3      13        8      10
    ## 251            #100                     2       4      12        9      11
    ## 252           #4/84                     2       3      13        6      10
    ## 253            #108                     2       3      13        6       8
    ## 254            #108                     2       3      14        6       8
    ## 255            #108                     2       3      13        6      10
    ## 256             #99                     2       .      13        7       9
    ## 257            #110                     2       .      12        7       9
    ## 258            #110                     2       .      12        6       8
    ## 259            #110                     2       .      12        7       9
    ## 260            #110                     2       .      12        7       9
    ## 261            #110                     2       .      12        7       9
    ## 262             #97                     1       3      12        7       9
    ## 263             #97                     1       3      12        6       8
    ## 264             #97                     1       3      12        6       9
    ## 265             #97                     1       3      12        7       9
    ## 266             #97                     1       3      12        7       9
    ## 267             #97                     1       3      12        7       9
    ## 268           #5/93                     1       3      12        8      10
    ## 269           #5/93                     1       3      13        7       9
    ## 270           #5/93                     1       3      13        7       9
    ## 271         #5/93/2                     1       4      13        7       9
    ## 272       #7/82/3-2                     1       3      12        6       8
    ## 273       #7/82/3-2                     1       4      13        5       8
    ## 274       #7/82/3-2                     1       3      13        6       8
    ## 275      #7/110/3-2                     1       3      14        8      10
    ## 276      #7/110/3-2                     1       3      14        8      10
    ## 277      #7/110/3-2                     1       4      14        8      10
    ## 278      #7/110/3-2                     1       3      14        7      10
    ## 279      #7/110/3-2                     1       3      14        8      10
    ## 280      #7/110/3-2                     1       3      14        8      10
    ## 281         #2/95/2                     1       4      13        7       9
    ## 282         #2/95/2                     1       4      13        7       9
    ## 283         #2/95/2                     1       4      13        7       9
    ## 284           #82/4                     1       4      13        8      10
    ## 285           #82/4                     1       3      13        7       9
    ## 286           #82/4                     1       4      13        7       9
    ## 287             #97                     2       3      12        7       9
    ## 288             #97                     2       3      12        6       8
    ## 289           #5/93                     2       4      13        7       9
    ## 290           #5/93                     2       3      12        7       9
    ## 291           #5/93                     2       4      13        7       9
    ## 292           #5/93                     2       3      13        7       9
    ## 293           #5/93                     2       3      12        7       9
    ## 294           #5/93                     2       3      12        7       9
    ## 295         #5/93/2                     2       4      14        7       9
    ## 296         #5/93/2                     2       5      14        7       9
    ## 297         #5/93/2                     2       4      13        7       9
    ## 298         #5/93/2                     2       5      14        7       9
    ## 299         #5/93/2                     2       4      13        7       9
    ## 300         #5/93/2                     2       4      14        6       9
    ## 301         #5/93/2                     2       5      13        7       9
    ## 302       #7/82/3-2                     2       3      13        6       8
    ## 303       #7/82/3-2                     2       3      12        6       8
    ## 304       #7/82/3-2                     2       3      12        6       8
    ## 305       #7/82/3-2                     2       3      12        6       8
    ## 306      #7/110/3-2                     2       4      14        8      10
    ## 307      #7/110/3-2                     2       4      14        7       9
    ## 308         #2/95/2                     2       4      12        7       9
    ## 309         #2/95/2                     2       4      12        6       8
    ## 310         #2/95/2                     2       4      13        7       9
    ## 311         #2/95/2                     2       4      12        7       9
    ## 312         #2/95/2                     2       3      13        6       8
    ## 313         #2/95/2                     2       3      13        7       9
    ## 314           #82/4                     2       4      13        7       9
    ## 315           #82/4                     2       3      13        7       9
    ## 316           #82/4                     2       3      13        7       9
