---
title: Hi Everyone
author: R package build
date: '2021-05-12'
slug: hi-everyone
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-05-12T21:46:26-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

# This is my first website. It is a work in progress...

## Experiment with color on gapminder data.


```r
# Note that I have already taken care of installing these in this RStudio Cloud project, but if you want to use these libraries yourself, here's how you'd install them:
library(devtools)
devtools::install_github("wilkelab/cowplot")
devtools::install_github("clauswilke/colorblindr")
devtools::install_github("dill/beyonce")
```


```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.1.1     ✓ dplyr   1.0.6
## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "numericVector" of class "Mnumeric"; definition not updated
```

```
## here() starts at /Users/looc/Box/2021_Spring_classes/Data_Viz/blogdown/Chris_Loo_website
```

```
## Registered S3 method overwritten by 'beyonce':
##   method        from       
##   print.palette wesanderson
```

```
## Loading required package: viridisLite
```

```
## Rows: 1,704
## Columns: 6
## $ country   <fct> "Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", …
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, …
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992, 1997, …
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.854, 40.8…
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 14880372, 12…
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 786.1134, …
```

## Version with _good_ color

Also, good with default colors


```r
brewer.pal(5, "Dark2") # list 5 hex colors
```

```
## [1] "#1B9E77" "#D95F02" "#7570B3" "#E7298A" "#66A61E"
```

```r
my_plot <- ggplot(gap, aes(x = log(gdpPercap), y = lifeExp, color = (continent))) + 
  geom_point(alpha = .2, size = 1) +
  labs(x = "GDP Per Capita (log)", y = "Life Expectancy") +
  facet_wrap(~continent)
my_plot + scale_color_brewer(palette = "Dark2")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

## Version with greyscale


```r
my_plot + scale_color_grey(start = 0.2, end = .8) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />
