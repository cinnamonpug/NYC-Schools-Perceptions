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
