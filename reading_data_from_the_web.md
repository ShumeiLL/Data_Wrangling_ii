reading_data_from_the_web
================
Shumei Liu
2024-10-10

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
```

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
drug_use_html = read_html(url)

drug_use_html
```

    ## {html_document}
    ## <html lang="en">
    ## [1] <head>\n<link rel="P3Pv1" href="http://www.samhsa.gov/w3c/p3p.xml">\n<tit ...
    ## [2] <body>\r\n\r\n<noscript>\r\n<p>Your browser's Javascript is off. Hyperlin ...

Get the pieces U actually need.

``` r
marj_use_df = 
  drug_use_html |>
  html_table() |>
  first() |>
  slice(-1)

marj_use_df
```

    ## # A tibble: 56 × 16
    ##    State     `12+(2013-2014)` `12+(2014-2015)` `12+(P Value)` `12-17(2013-2014)`
    ##    <chr>     <chr>            <chr>            <chr>          <chr>             
    ##  1 Total U.… 12.90a           13.36            0.002          13.28b            
    ##  2 Northeast 13.88a           14.66            0.005          13.98             
    ##  3 Midwest   12.40b           12.76            0.082          12.45             
    ##  4 South     11.24a           11.64            0.029          12.02             
    ##  5 West      15.27            15.62            0.262          15.53a            
    ##  6 Alabama   9.98             9.60             0.426          9.90              
    ##  7 Alaska    19.60a           21.92            0.010          17.30             
    ##  8 Arizona   13.69            13.12            0.364          15.12             
    ##  9 Arkansas  11.37            11.59            0.678          12.79             
    ## 10 Californ… 14.49            15.25            0.103          15.03             
    ## # ℹ 46 more rows
    ## # ℹ 11 more variables: `12-17(2014-2015)` <chr>, `12-17(P Value)` <chr>,
    ## #   `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>, `18-25(P Value)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `26+(P Value)` <chr>,
    ## #   `18+(2013-2014)` <chr>, `18+(2014-2015)` <chr>, `18+(P Value)` <chr>

Read in cost of living data.

``` r
url = "https://www.bestplaces.net/cost_of_living/city/new_york/new_york"
cost_used_html = read_html(url)

cost_used_html
```

    ## {html_document}
    ## <html xmlns="//www.w3.org/1999/xhtml">
    ## [1] <head>\n<script async src="https://pagead2.googlesyndication.com/pagead/j ...
    ## [2] <body><form method="post" action="/cost_of_living/city/new_york/new_york? ...

``` r
nyc_cost_df = 
  cost_used_html |>
  html_table()

nyc_cost_df
```

    ## [[1]]
    ## # A tibble: 9 × 4
    ##   X1               X2       X3       X4           
    ##   <chr>            <chr>    <chr>    <chr>        
    ## 1 Categories       New York New York United States
    ## 2 Overall          172.5    121.5    100.0        
    ## 3 Grocery          116.6    103.8    100          
    ## 4 Health           127.6    120.7    100.0        
    ## 5 Housing          224.3    127.9    100.0        
    ## 6 Median Home Cost $677,200 $413,600 $338,100     
    ## 7 Utilities        150.5%   115.9%   100.0%       
    ## 8 Transportation   181.1    140.7    100.0        
    ## 9 Miscellaneous    136.4    121.8    100.0

or can write in

``` r
nyc_cost_df = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") |>
  html_table(header = TRUE) |>
  first()

nyc_cost_df
```

    ## # A tibble: 8 × 4
    ##   Categories       `New York` `New York` `United States`
    ##   <chr>            <chr>      <chr>      <chr>          
    ## 1 Overall          172.5      121.5      100.0          
    ## 2 Grocery          116.6      103.8      100            
    ## 3 Health           127.6      120.7      100.0          
    ## 4 Housing          224.3      127.9      100.0          
    ## 5 Median Home Cost $677,200   $413,600   $338,100       
    ## 6 Utilities        150.5%     115.9%     100.0%         
    ## 7 Transportation   181.1      140.7      100.0          
    ## 8 Miscellaneous    136.4      121.8      100.0
