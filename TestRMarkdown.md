My Test R Markdown
================
AA
31/05/2020

The dataset is publicly available at: combined.csv:
<https://data.world/dataquest/nyc-schools-data/workspace/file?filename=combined.csv>
NYC school survey data for 2011:
<https://data.cityofnewyork.us/Education/2011-NYC-School-Survey/mnz3-dyi8>

We will try to answer the following questions:

Do student, teacher, and parent perceptions of NYC school quality appear
to be related to demographic and academic success metrics?

Do students, teachers, and parents have similar perceptions of NYC
school quality?

First, let’s load the required libraries:

``` r
library(readr)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(purrr)
library(ggplot2)
library(stringr)
library(tidyr)
```

Let’s import the data that we cleaned from the previous courses.

``` r
combined <- read_csv("combined.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   DBN = col_character(),
    ##   school_name = col_character(),
    ##   boro = col_character()
    ## )

    ## See spec(...) for full column specifications.

Next, we’ll import the survey data. The txt survey data is separated by
tabs so we will use read\_tsv() from the readr library to import the
files masterfile11\_gened\_final.txt and masterfile11\_d75\_final.txt

``` r
masterfile1_gened <- read_tsv("masterfile11_gened_final.txt")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   dbn = col_character(),
    ##   bn = col_character(),
    ##   schoolname = col_character(),
    ##   studentssurveyed = col_character(),
    ##   schooltype = col_character(),
    ##   p_q1 = col_logical(),
    ##   p_q3d = col_logical(),
    ##   p_q9 = col_logical(),
    ##   p_q10 = col_logical(),
    ##   p_q12aa = col_logical(),
    ##   p_q12ab = col_logical(),
    ##   p_q12ac = col_logical(),
    ##   p_q12ad = col_logical(),
    ##   p_q12ba = col_logical(),
    ##   p_q12bb = col_logical(),
    ##   p_q12bc = col_logical(),
    ##   p_q12bd = col_logical(),
    ##   t_q6m = col_logical(),
    ##   t_q9 = col_logical(),
    ##   t_q10a = col_logical()
    ##   # ... with 18 more columns
    ## )

    ## See spec(...) for full column specifications.

``` r
masterfile1_d75 <- read_tsv("masterfile11_d75_final.txt")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   dbn = col_character(),
    ##   bn = col_character(),
    ##   schoolname = col_character(),
    ##   studentssurveyed = col_character(),
    ##   schooltype = col_character(),
    ##   p_q5 = col_logical(),
    ##   p_q9 = col_logical(),
    ##   p_q13a = col_logical(),
    ##   p_q13b = col_logical(),
    ##   p_q13c = col_logical(),
    ##   p_q13d = col_logical(),
    ##   p_q14a = col_logical(),
    ##   p_q14b = col_logical(),
    ##   p_q14c = col_logical(),
    ##   p_q14d = col_logical(),
    ##   t_q11a = col_logical(),
    ##   t_q11b = col_logical(),
    ##   t_q14 = col_logical(),
    ##   t_q15a = col_logical(),
    ##   t_q15b = col_logical()
    ##   # ... with 14 more columns
    ## )
    ## See spec(...) for full column specifications.

Let’s check out the data that we’ve imported:

``` r
head(masterfile1_gened,5)
```

    ## # A tibble: 5 x 1,942
    ##   dbn   bn    schoolname   d75 studentssurveyed highschool schooltype  rr_s
    ##   <chr> <chr> <chr>      <dbl> <chr>                 <dbl> <chr>      <dbl>
    ## 1 01M0~ M015  P.S. 015 ~     0 No                        0 Elementar~    NA
    ## 2 01M0~ M019  P.S. 019 ~     0 No                        0 Elementar~    NA
    ## 3 01M0~ M020  P.S. 020 ~     0 No                        0 Elementar~    NA
    ## 4 01M0~ M034  P.S. 034 ~     0 Yes                       0 Elementar~    89
    ## 5 01M0~ M063  P.S. 063 ~     0 No                        0 Elementar~    NA
    ## # ... with 1,934 more variables: rr_t <dbl>, rr_p <dbl>, N_s <dbl>, N_t <dbl>,
    ## #   N_p <dbl>, nr_s <dbl>, nr_t <dbl>, nr_p <dbl>, saf_p_11 <dbl>,
    ## #   com_p_11 <dbl>, eng_p_11 <dbl>, aca_p_11 <dbl>, saf_t_11 <dbl>,
    ## #   com_t_11 <dbl>, eng_t_11 <dbl>, aca_t_11 <dbl>, saf_s_11 <dbl>,
    ## #   com_s_11 <dbl>, eng_s_11 <dbl>, aca_s_11 <dbl>, saf_tot_11 <dbl>,
    ## #   com_tot_11 <dbl>, eng_tot_11 <dbl>, aca_tot_11 <dbl>, p_q2h <dbl>,
    ## #   p_q7a <dbl>, p_q7b <dbl>, p_q7c <dbl>, p_q7d <dbl>, p_q8a <dbl>,
    ## #   p_q8b <dbl>, p_q8c <dbl>, p_q8d <dbl>, p_q8e <dbl>, p_q8f <dbl>,
    ## #   p_q2b <dbl>, p_q2d <dbl>, p_q2e <dbl>, p_q2f <dbl>, p_q2g <dbl>,
    ## #   p_q3a <dbl>, p_q3b <dbl>, p_q4b <dbl>, p_q4c <dbl>, p_q11c <dbl>,
    ## #   p_q2a <dbl>, p_q2c <dbl>, p_q3c <dbl>, p_q6a <dbl>, p_q6b <dbl>,
    ## #   p_q11d <dbl>, p_q11e <dbl>, p_q5 <dbl>, p_q4a <dbl>, p_q4d <dbl>,
    ## #   p_q4e <dbl>, p_q11a <dbl>, p_q11b <dbl>, p_q11f <dbl>, p_q1 <lgl>,
    ## #   p_q3d <lgl>, p_q9 <lgl>, p_q10 <lgl>, p_q12aa <lgl>, p_q12ab <lgl>,
    ## #   p_q12ac <lgl>, p_q12ad <lgl>, p_q12ba <lgl>, p_q12bb <lgl>, p_q12bc <lgl>,
    ## #   p_q12bd <lgl>, p_q1_1 <dbl>, p_q1_2 <dbl>, p_q1_3 <dbl>, p_q1_4 <dbl>,
    ## #   p_q1_5 <dbl>, p_q1_6 <dbl>, p_q1_7 <dbl>, p_q1_8 <dbl>, p_q1_9 <dbl>,
    ## #   p_q1_10 <dbl>, p_q1_11 <dbl>, p_q1_12 <dbl>, p_q1_13 <dbl>, p_q1_14 <dbl>,
    ## #   p_q2a_1 <dbl>, p_q2a_2 <dbl>, p_q2a_3 <dbl>, p_q2a_4 <dbl>, p_q2a_5 <dbl>,
    ## #   p_q2b_1 <dbl>, p_q2b_2 <dbl>, p_q2b_3 <dbl>, p_q2b_4 <dbl>, p_q2b_5 <dbl>,
    ## #   p_q2c_1 <dbl>, p_q2c_2 <dbl>, p_q2c_3 <dbl>, p_q2c_4 <dbl>, p_q2c_5 <dbl>,
    ## #   ...

``` r
head(masterfile1_d75,5)
```

    ## # A tibble: 5 x 1,773
    ##   dbn   bn    schoolname   d75 studentssurveyed highschool schooltype  rr_s
    ##   <chr> <chr> <chr>      <dbl> <chr>                 <dbl> <chr>      <dbl>
    ## 1 75K0~ K004  P.S. K004      1 Yes                       0 District ~    38
    ## 2 75K0~ K036  P.S. 36        1 Yes                      NA District ~    70
    ## 3 75K0~ K053  P.S. K053      1 Yes                      NA District ~    94
    ## 4 75K0~ K077  P.S. K077      1 Yes                      NA District ~    95
    ## 5 75K1~ K140  P.S. K140      1 Yes                       0 District ~    77
    ## # ... with 1,765 more variables: rr_t <dbl>, rr_p <dbl>, N_s <dbl>, N_t <dbl>,
    ## #   N_p <dbl>, nr_s <dbl>, nr_t <dbl>, nr_p <dbl>, saf_p_11 <dbl>,
    ## #   com_p_11 <dbl>, eng_p_11 <dbl>, aca_p_11 <dbl>, saf_t_11 <dbl>,
    ## #   com_t_11 <dbl>, eng_t_11 <dbl>, aca_t_11 <dbl>, saf_s_11 <dbl>,
    ## #   com_s_11 <dbl>, eng_s_11 <dbl>, aca_s_11 <dbl>, saf_tot_11 <dbl>,
    ## #   com_tot_11 <dbl>, eng_tot_11 <dbl>, aca_tot_11 <dbl>, p_q1c <dbl>,
    ## #   p_q10a <dbl>, p_q10b <dbl>, p_q10c <dbl>, p_q10d <dbl>, p_q10e <dbl>,
    ## #   p_q10f <dbl>, p_q11a <dbl>, p_q11b <dbl>, p_q11c <dbl>, p_q11d <dbl>,
    ## #   p_q11e <dbl>, p_q1b <dbl>, p_q1e <dbl>, p_q1f <dbl>, p_q2a <dbl>,
    ## #   p_q2b <dbl>, p_q3c <dbl>, p_q3d <dbl>, p_q4a <dbl>, p_q6c <dbl>,
    ## #   p_q12c <dbl>, p_q1a <dbl>, p_q1d <dbl>, p_q3a <dbl>, p_q3b <dbl>,
    ## #   p_q3e <dbl>, p_q4b <dbl>, p_q4c <dbl>, p_q6e <dbl>, p_q7 <dbl>,
    ## #   p_q8a <dbl>, p_q8b <dbl>, p_q12d <dbl>, p_q1g <dbl>, p_q6a <dbl>,
    ## #   p_q6b <dbl>, p_q6d <dbl>, p_q6f <dbl>, p_q6g <dbl>, p_q6h <dbl>,
    ## #   p_q12a <dbl>, p_q12b <dbl>, p_q12e <dbl>, p_q12f <dbl>, p_q5 <lgl>,
    ## #   p_q9 <lgl>, p_q13a <lgl>, p_q13b <lgl>, p_q13c <lgl>, p_q13d <lgl>,
    ## #   p_q14a <lgl>, p_q14b <lgl>, p_q14c <lgl>, p_q14d <lgl>, p_q1a_1 <dbl>,
    ## #   p_q1a_2 <dbl>, p_q1a_3 <dbl>, p_q1a_4 <dbl>, p_q1a_5 <dbl>, p_q1b_1 <dbl>,
    ## #   p_q1b_2 <dbl>, p_q1b_3 <dbl>, p_q1b_4 <dbl>, p_q1b_5 <dbl>, p_q1c_1 <dbl>,
    ## #   p_q1c_2 <dbl>, p_q1c_3 <dbl>, p_q1c_4 <dbl>, p_q1c_5 <dbl>, p_q1d_1 <dbl>,
    ## #   p_q1d_2 <dbl>, p_q1d_3 <dbl>, p_q1d_4 <dbl>, p_q1d_5 <dbl>, p_q1e_1 <dbl>,
    ## #   ...
