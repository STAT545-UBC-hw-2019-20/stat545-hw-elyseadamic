---
title: "Homework 2: Exercise 1"
author: "Elyse Adamic"
output:
  html_document:
    keep_md: true
    toc: true
    toc_depth: 3
---

## Exercise 1: Basic Data Wrangling

 
### 1.1 Display the data for 3 countries in the 1970s:

```r
#levels(gapminder$country) to choose countries
#unique(gapminder$year) to see years

# There are 143 countries to choose form, and only 2 years in the 1970s
gapminder %>%
  filter(country == "Canada" | country == "Switzerland" | country == "Sweden") %>%
  filter(year < 1980 & year >= 1970)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Canada      Americas   1972    72.9 22284500    18971.
## 2 Canada      Americas   1977    74.2 23796400    22091.
## 3 Sweden      Europe     1972    74.7  8122293    17832.
## 4 Sweden      Europe     1977    75.4  8251648    18856.
## 5 Switzerland Europe     1972    73.8  6401400    27195.
## 6 Switzerland Europe     1977    75.4  6316424    26982.
```

### 1.2 Select specific variables 

```r
gapminder %>%
  filter(country == "Canada" | country == "Switzerland" | country == "Sweden") %>%
  filter(year < 1980 & year >= 1970) %>% 
  select(country, gdpPercap)
```

```
## # A tibble: 6 x 2
##   country     gdpPercap
##   <fct>           <dbl>
## 1 Canada         18971.
## 2 Canada         22091.
## 3 Sweden         17832.
## 4 Sweden         18856.
## 5 Switzerland    27195.
## 6 Switzerland    26982.
```
 
### 1.3 Which entries have experienced a drop in life expectancy?

```r
gapminder %>%
  arrange(country, year) %>% 
  group_by(country) %>%
  mutate(incr_lifeExp = lifeExp - lag(lifeExp)) %>% 
  filter(incr_lifeExp < 0)
```

```
## # A tibble: 102 x 7
## # Groups:   country [52]
##    country  continent  year lifeExp     pop gdpPercap incr_lifeExp
##    <fct>    <fct>     <int>   <dbl>   <int>     <dbl>        <dbl>
##  1 Albania  Europe     1992    71.6 3326498     2497.       -0.419
##  2 Angola   Africa     1987    39.9 7874230     2430.       -0.036
##  3 Benin    Africa     2002    54.4 7026113     1373.       -0.371
##  4 Botswana Africa     1992    62.7 1342614     7954.       -0.877
##  5 Botswana Africa     1997    52.6 1536536     8647.      -10.2  
##  6 Botswana Africa     2002    46.6 1630347    11004.       -5.92 
##  7 Bulgaria Europe     1977    70.8 8797022     7612.       -0.09 
##  8 Bulgaria Europe     1992    71.2 8658506     6303.       -0.15 
##  9 Bulgaria Europe     1997    70.3 8066057     5970.       -0.87 
## 10 Burundi  Africa     1992    44.7 5809236      632.       -3.48 
## # … with 92 more rows
```

### 1.4 Show the max gdp for each country. 

```r
gapminder %>%
  group_by(country) %>%
  filter(gdpPercap == max(gdpPercap))
```

```
## # A tibble: 142 x 6
## # Groups:   country [142]
##    country     continent  year lifeExp       pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Afghanistan Asia       1982    39.9  12881816      978.
##  2 Albania     Europe     2007    76.4   3600523     5937.
##  3 Algeria     Africa     2007    72.3  33333216     6223.
##  4 Angola      Africa     1967    36.0   5247469     5523.
##  5 Argentina   Americas   2007    75.3  40301927    12779.
##  6 Australia   Oceania    2007    81.2  20434176    34435.
##  7 Austria     Europe     2007    79.8   8199783    36126.
##  8 Bahrain     Asia       2007    75.6    708573    29796.
##  9 Bangladesh  Asia       2007    64.1 150448339     1391.
## 10 Belgium     Europe     2007    79.4  10392226    33693.
## # … with 132 more rows
```

### 1.5 Life expectancy vs. log GDP per capita in Canada

```r
gapminder %>% 
  filter(country == "Canada") %>% 
  ggplot(aes(gdpPercap,lifeExp)) +
  geom_point() + 
  scale_x_log10("GDP Per Capita") +
  ylab("Life Expectancy")
```

![](hw02_exercise1_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
