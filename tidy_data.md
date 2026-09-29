Tidy Data
================
Emma
2026-09-29

This file is for doing data tidying.

``` r
library(tidyverse)
```

# Let’s tidy some data!

``` r
pulse_df = 
  haven::read_sas("data_import_examples/public_pulse_data.sas7bdat") |> 
  janitor::clean_names()
```

This data is not tidy! Visit information (eg. 6 mo) embedded into column
names, important information also embedded

# Let’s tidy

``` r
pulse_tidy_df = 
  pulse_df |> 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi_score"
  ) |> 
  mutate(
    visit = replace(visit, visit == "bl", "00m")
  )
```

Say which columns you’re tidying, the other columns will get duplicated.
The column names will go into a new variable, called visit. There’s a
prefix there you don’t need, so remove that (bdi_score\_) The values
will go to a new columns called bdi_score Then, replace bl with 00m, to
make it all the same format using mutate function. visit is column name

# Let’s practice

Import the litters data; keep columns litter number and GD weights; and
tidy

``` r
litters_df = 
  read_csv("data_import_examples/FAS_litters.csv", na = c("", ".", "NA")) |> 
  janitor::clean_names() |> 
  select(litter_number, gd0_weight, gd18_weight) |> 
  pivot_longer(
    gd0_weight:gd18_weight, 
    names_to = "gd", 
    values_to = "weight"
  ) |> 
  mutate(
    gd = case_match(
      gd, 
     "gd0_weight" ~ 0, 
     "gd18_weight" ~ 18,
    )
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

    ## Warning: There was 1 warning in `mutate()`.
    ## ℹ In argument: `gd = case_match(gd, "gd0_weight" ~ 0, "gd18_weight" ~ 18, )`.
    ## Caused by warning:
    ## ! `case_match()` was deprecated in dplyr 1.2.0.
    ## ℹ Please use `recode_values()` instead.

No names suffix, so use replace function to clean up variable names
Mutate - column names/columns need to be modified. Say which column am I
working on, and what is the new name (we just replaced gd with gd). then
list out cases and what you want to replace them with. Here we replace
gd0_weight with 0.
