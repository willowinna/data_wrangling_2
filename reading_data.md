reading_data
================

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## 载入程序包：'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

``` r
url = "https://www.samhsa.gov/data/sites/default/files/NSDUHsaeShortTermCHG2015/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)

drug_use_html
```

    ## {html_document}
    ## <html lang="en">
    ## [1] <head>\n<title>National Survey on Drug Use and Health: Comparison of 2013 ...
    ## [2] <body>\r\n\r\n<noscript>\r\n<p>Your browser's Javascript is off. Hyperlin ...

pull the tables out

``` r
marj_df =
  drug_use_html |>
  html_table() |>
  first() |>  #first table
  slice(-1)  #delete the first row
```

import starwars data

``` r
swm_html = 
  read_html_live("https://www.imdb.com/list/ls070150896/")
```

``` r
title_vec = 
  swm_html |>
  html_elements(".ipc-title-link-wrapper .ipc-title__text") |>
  html_text()

metascore_vec = 
  swm_html |>
  html_elements(".metacritic-score-box") |>
  html_text()

runtime_vec = 
  swm_html |>
  html_elements(".base .ipc-inline-list__item:nth-child(2)") |>
  html_text()

swm_df = 
  tibble(
    title = title_vec,
    score = metascore_vec,
    runtime = runtime_vec)
```

0000000

\##use apis

get a csv

``` r
nyc_water_df = 
  GET("https://data.cityofnewyork.us/api/v3/views/ia2d-e54m/query.csv") |> 
  content("parsed") |>
  select(year:per_capita_gallons_per_person_per_day)
```

    ## Rows: 65 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): :id, :version
    ## dbl  (4): year, new_york_city_population, nyc_consumption_million_gallons_pe...
    ## dttm (2): :created_at, :updated_at
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

get a json file

``` r
nyc_water = 
  GET("https://data.cityofnewyork.us/api/v3/views/ia2d-e54m/query.json") |> 
  content("text") |>
  jsonlite::fromJSON() |>
  as_tibble() |>
  select(year:per_capita_gallons_per_person_per_day)
```

pokemon

``` r
pokemon_json= 
  GET("http://pokeapi.co/api/v2/pokemon/668") |>
  content()
```
