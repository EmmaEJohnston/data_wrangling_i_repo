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

# Deliberately untidy data

``` r
analysis_df = 
  tibble(
    groups = c("treatment", "treatment", "placebo", "placebo"), 
    time = c("pre", "post", "pre", "post"), 
    mean_outcome = c(4, 8, 3.5, 4.6) 
  )
## this is a tidy way to show data. but if we were showing data to someone else, we might want to untidy for human readibility 
```

# Untidy our new df

``` r
analysis_df |> 
  pivot_wider(
    names_from = time, 
    values_from = mean_outcome
  ) |> 
  knitr::kable() ## formats nice table for an Rmd doc 
```

| groups    | pre | post |
|:----------|----:|-----:|
| treatment | 4.0 |  8.0 |
| placebo   | 3.5 |  4.6 |

names come from time variable, giving you columns for pre and post. The
values come from mean outcome column and go into this new column

# bind some rows

Import each LotR movie table

``` r
fellowship_df = 
  readxl::read_excel("data_import_examples/LotR_Words.xlsx", range = "B3:D6") |> 
  mutate(movie = "fellowship")

two_towers_df = 
  readxl::read_excel("data_import_examples/LotR_Words.xlsx", range = "F3:H6") |> 
  mutate(movie = "two towers")

return_df = 
  readxl::read_excel("data_import_examples/LotR_Words.xlsx", range = "J3:L6") |> 
  mutate(movie = "return of the king")
```

Put all of the new dfs together, and do all the tidying afterwards. that
way don’t have to do three times.

``` r
lotr_df = 
  bind_rows(fellowship_df, two_towers_df, return_df) |> 
  janitor::clean_names() |> 
  relocate(movie) |> 
  pivot_longer(
    female:male, 
    names_to = "gender", 
    values_to = "words"
  )
```

binding rows stacks rows on top of each other. relocate movie column to
far left. pivot longer to tidy data.

# Join FAS datasets (bring data from one dataset into another dataset)

``` r
pups_df = 
  read_csv(
    "data_import_examples/FAS_pups.csv", 
    skip = 3, 
    na = c("", ".", "NA")) |> 
  janitor::clean_names() |> 
  mutate(
    sex = case_match(
      sex, 
      1 ~ "male", 
      2 ~ "female"
    ) # renaming values from 1/2 to male/female in df 
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
litters_df = 
  read_csv(
    "data_import_examples/FAS_litters.csv", 
    na = c("", ".", "NA")) |> 
  janitor::clean_names() |> 
  relocate(litter_number) |> 
  separate(group, into = c("dose", "day_of_tx"), 3) |> 
  mutate(
    dose = str_to_lower(dose), 
    day_of_tx = as.numeric(day_of_tx), 
    gd_weight_gain = gd18_weight - gd0_weight
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
#which column are you separating, and what are you separating into. 3 is first three characters is where you split/separate variable name  

fas_df =
  left_join(pups_df, litters_df, by = "litter_number")
```

joining stuff from litters df into pups df. litter number exists in
both, so say by = to say which is ID variable (that’s the point at which
you combine). in this case, we’re going to fix issues in pups and
litters df separately, and then re-join because you might want to use
pups and litters separately at some point.
