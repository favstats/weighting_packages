Weighting Packages
================

  - [{srvyr}](https://github.com/gergness/srvyr)
  - [{TAM}](https://www.rdocumentation.org/packages/TAM/versions/2.12-18/topics/weighted_Stats)
  - [{Weighted.Desc.Stat}](https://rdrr.io/cran/Weighted.Desc.Stat/man/)
  - [{questionr}](https://juba.github.io/questionr/reference/index.html)
  - [{sjstats}](https://www.rdocumentation.org/packages/sjstats/versions/0.17.0/topics/weight)

<!-- end list -->

``` r
pacman::p_load(srvyr, TAM, Weighted.Desc.Stat, questionr, sjstats, tidyverse)
```

## Data

``` r
load("data/ess1_8.Rdata")
```

## Examples

### srvyr

``` r
ess1_8 %>%
   srvyr::as_survey_design(weights = pweight) %>% 
   summarise(imueclt = srvyr::survey_mean(imueclt, na.rm = T)) 
```

    ## # A tibble: 1 x 2
    ##   imueclt imueclt_se
    ##     <dbl>      <dbl>
    ## 1    5.20    0.00698

### TAM

``` r
ess1_8 %>% 
  summarise(skew = TAM::weighted_skewness(x = imueclt, w = pweight),
            kurt = TAM::weighted_curtosis(x = imueclt, w = pweight))
```

    ## # A tibble: 1 x 2
    ##     skew   kurt
    ##    <dbl>  <dbl>
    ## 1 -0.207 -0.668

### Weighted.Desc.Stat

``` r
ess1_8 %>% 
  drop_na(imueclt, pweight) %>% 
  summarise(skew = Weighted.Desc.Stat::w.skewness(x = imueclt, mu = pweight),
            kurt = Weighted.Desc.Stat::w.kurtosis(x = imueclt, mu = pweight))
```

    ## # A tibble: 1 x 2
    ##     skew   kurt
    ##    <dbl>  <dbl>
    ## 1 -0.207 -0.668

### questionr

``` r
questionr::wtd.table(ess1_8$imueclt, weights = ess1_8$pweight) %>% 
  as_tibble()
```

    ## # A tibble: 11 x 2
    ##    Var1       n
    ##    <chr>  <dbl>
    ##  1 0     24240.
    ##  2 1     15588.
    ##  3 2     25171.
    ##  4 3     32463.
    ##  5 4     30309.
    ##  6 5     73251.
    ##  7 6     37839.
    ##  8 7     47782.
    ##  9 8     43218.
    ## 10 9     16053.
    ## 11 10    20288.

### sjstats

**DOESN’T WORK**

``` r
ess1_8 %>%
  drop_na(imueclt) %>% 
   mutate(imueclt = sjstats::weight2(imueclt, pweight))
```
