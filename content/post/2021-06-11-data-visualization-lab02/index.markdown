---
title: Data Visualization Lab02
author: Christopher P. Loo
date: '2021-06-11'
slug: data-visualization-lab02
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-06-11T22:56:40-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


# Workflow for Lab 02

The lab instructions can be found [here](https://stevenbedrick.github.io/data-vis-labs-2020/02-moma.html); we will work through its contents together via Webex. You will use this RMarkdown file as your workspace and final document. Don't forget to update the "author" metadata field at the top of the file!

# Start by Loading Libraries




```r
moma <- read_csv(here::here("content","post","2021-06-11-data-visualization-lab02","data", "artworks-cleaned.csv"))
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   title = col_character(),
##   artist = col_character(),
##   artist_bio = col_character(),
##   artist_gender = col_character(),
##   circumference_cm = col_logical(),
##   diameter_cm = col_logical(),
##   length_cm = col_logical(),
##   seat_height_cm = col_logical(),
##   purchase = col_logical(),
##   gift = col_logical(),
##   exchange = col_logical(),
##   classification = col_character(),
##   department = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```


```r
glimpse(moma)
```

```
## Rows: 2,253
## Columns: 23
## $ title             <chr> "Rope and People, I", "Fire in the Evening", "Portra…
## $ artist            <chr> "Joan Miró", "Paul Klee", "Paul Klee", "Pablo Picass…
## $ artist_bio        <chr> "(Spanish, 1893–1983)", "(German, born Switzerland. …
## $ artist_birth_year <dbl> 1893, 1879, 1879, 1881, 1880, 1879, 1943, 1880, 1839…
## $ artist_death_year <dbl> 1983, 1940, 1940, 1973, 1946, 1953, 1977, 1950, 1906…
## $ num_artists       <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
## $ n_female_artists  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
## $ n_male_artists    <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1…
## $ artist_gender     <chr> "Male", "Male", "Male", "Male", "Male", "Male", "Mal…
## $ year_acquired     <dbl> 1936, 1970, 1966, 1955, 1939, 1968, 1997, 1931, 1934…
## $ year_created      <dbl> 1935, 1929, 1927, 1919, 1925, 1919, 1970, 1929, 1885…
## $ circumference_cm  <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ depth_cm          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ diameter_cm       <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ height_cm         <dbl> 104.8, 33.8, 60.3, 215.9, 50.8, 129.2, 200.0, 54.6, …
## $ length_cm         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ width_cm          <dbl> 74.6, 33.3, 36.8, 78.7, 54.0, 89.9, 200.0, 38.1, 96.…
## $ seat_height_cm    <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, …
## $ purchase          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FAL…
## $ gift              <lgl> TRUE, FALSE, FALSE, TRUE, TRUE, FALSE, TRUE, TRUE, F…
## $ exchange          <lgl> FALSE, FALSE, FALSE, FALSE, TRUE, FALSE, FALSE, FALS…
## $ classification    <chr> "Painting", "Painting", "Painting", "Painting", "Pai…
## $ department        <chr> "Painting & Sculpture", "Painting & Sculpture", "Pai…
```


# Know Your Data

## How Many Paintings?
2,253

## What is the first painting that was acquired?

```r
moma %>% select(artist, title, year_acquired) %>% 
  arrange(year_acquired)
```

```
## # A tibble: 2,253 x 3
##    artist           title                                          year_acquired
##    <chr>            <chr>                                                  <dbl>
##  1 Edward Hopper    House by the Railroad                                   1930
##  2 Bernard Karfiol  Seated Nude                                             1930
##  3 Pierre Roy       Daylight Savings Time                                   1931
##  4 Preston Dickins… Plums on a Plate                                        1931
##  5 Otto Dix         Dr. Mayer-Hermann                                       1932
##  6 Paul Cézanne     The Bather                                              1934
##  7 Paul Cézanne     Pines and Rocks (Fontainebleau?)                        1934
##  8 Paul Cézanne     Still Life with Ginger Jar, Sugar Bowl, and O…          1934
##  9 Paul Cézanne     Still Life with Apples                                  1934
## 10 Arthur B. Davies Italian Landscape                                       1934
## # … with 2,243 more rows
```
"House by the Railroad" by Edward Hopper and 
"Seated Nude" by Bernard Karfiol were both aquired in 1930

## What is the oldest painting?

```r
moma %>% select(artist, title, year_created) %>% 
  arrange(year_created)
```

```
## # A tibble: 2,253 x 3
##    artist       title                                    year_created
##    <chr>        <chr>                                           <dbl>
##  1 Odilon Redon Landscape at Daybreak                            1872
##  2 Odilon Redon Apache (Man on Horseback)                        1875
##  3 Odilon Redon Apache (Man on Horseback II)                     1875
##  4 Odilon Redon Fishing Boat                                     1875
##  5 Odilon Redon Rocky Peak                                       1875
##  6 Odilon Redon The Rocky Slope                                  1875
##  7 Odilon Redon Landscape with Rocks, near Royan                 1875
##  8 Paul Cézanne Still Life with Fruit Dish                       1879
##  9 Paul Cézanne L'Estaque                                        1879
## 10 Claude Monet On the Cliff at Pourville, Clear Weather         1882
## # … with 2,243 more rows
```
"Landscape at Daybreak" by Odilon Redon was made in 1872.

## How many artists?

```r
# distinct(moma, 'artist'), this only gives the number or columns for 'artist' which is 1
moma %>% distinct(artist)
```

```
## # A tibble: 989 x 1
##    artist           
##    <chr>            
##  1 Joan Miró        
##  2 Paul Klee        
##  3 Pablo Picasso    
##  4 Arthur Dove      
##  5 Francis Picabia  
##  6 Blinky Palermo   
##  7 Pierre Roy       
##  8 Paul Cézanne     
##  9 Enrico Prampolini
## 10 Jankel Adler     
## # … with 979 more rows
```
There are 989 distinct artists in this dataset.

## Which artist has the most paintings?

```r
# moma %>% distinct(artist) %>% count(title) does not work
moma %>% count(artist, sort = TRUE)
```

```
## # A tibble: 989 x 2
##    artist               n
##    <chr>            <int>
##  1 Pablo Picasso       55
##  2 Henri Matisse       32
##  3 On Kawara           32
##  4 Jacob Lawrence      30
##  5 Batiste Madalena    25
##  6 Jean Dubuffet       25
##  7 Odilon Redon        25
##  8 Ben Vautier         24
##  9 Frank Stella        23
## 10 Philip Guston       23
## # … with 979 more rows
```
Pablo Picasso has the most paintings in the moma dataset with 55 paintings

## How many paintings, by gender?

```r
moma %>% count(artist_gender, sort = TRUE)
```

```
## # A tibble: 3 x 2
##   artist_gender     n
##   <chr>         <int>
## 1 Male           1991
## 2 Female          252
## 3 <NA>             10
```

## How many artists, by gender?

```r
moma %>% count(artist_gender, artist, sort = TRUE) %>% 
  filter(artist_gender == "Female")
```

```
## # A tibble: 143 x 3
##    artist_gender artist                    n
##    <chr>         <chr>                 <int>
##  1 Female        Sherrie Levine           12
##  2 Female        Agnes Martin              9
##  3 Female        Elizabeth Murray          8
##  4 Female        Susan Rothenberg          8
##  5 Female        Joan Mitchell             6
##  6 Female        Loren MacIver             6
##  7 Female        R. H. Quaytman            6
##  8 Female        Helen Frankenthaler       5
##  9 Female        Georgia O'Keeffe          4
## 10 Female        Lynette Yiadom-Boakye     4
## # … with 133 more rows
```


```r
moma %>% count(artist_gender, artist, sort = TRUE) %>% 
  filter(artist_gender == "Male")
```

```
## # A tibble: 837 x 3
##    artist_gender artist               n
##    <chr>         <chr>            <int>
##  1 Male          Pablo Picasso       55
##  2 Male          Henri Matisse       32
##  3 Male          On Kawara           32
##  4 Male          Jacob Lawrence      30
##  5 Male          Batiste Madalena    25
##  6 Male          Jean Dubuffet       25
##  7 Male          Odilon Redon        25
##  8 Male          Ben Vautier         24
##  9 Male          Frank Stella        23
## 10 Male          Philip Guston       23
## # … with 827 more rows
```


```r
# I knew there was a better method, seems a bit weird to use count twice
moma %>% count(artist_gender, artist) %>% 
  count(artist_gender)
```

```
## # A tibble: 3 x 2
##   artist_gender     n
##   <chr>         <int>
## 1 Female          143
## 2 Male            837
## 3 <NA>              9
```

## In which years were the most paintings in the collection _acquired_?

```r
# moma %>% count(artist, year_acquired) %>% count(year_acquired, sort = TRUE)   # this gave me a different result, which I don't completely understand.

moma %>% count(year_acquired, sort = TRUE)
```

```
## # A tibble: 88 x 2
##    year_acquired     n
##            <dbl> <int>
##  1          1985    86
##  2          1942    71
##  3          1979    71
##  4          1991    67
##  5          2005    67
##  6          1967    65
##  7          2008    55
##  8          1961    45
##  9          1969    45
## 10          1956    42
## # … with 78 more rows
```
In 1985 there were 86 paintings that the MoMa acquired.

## In which years were the most paintings in the collection _created_?

```r
moma %>% count(year_created, sort = TRUE)
```

```
## # A tibble: 139 x 2
##    year_created     n
##           <dbl> <int>
##  1         1977    57
##  2         1940    56
##  3         1964    56
##  4         1961    50
##  5         1962    49
##  6         1963    44
##  7         1959    42
##  8         1968    40
##  9         1960    39
## 10         1914    37
## # … with 129 more rows
```
The collection has most paintings that were created in 1977.

## What about the first painting by a solo female artist?

```r
moma %>% filter(artist_gender == "Female") %>% 
  select(year_acquired) %>% arrange(year_acquired)
```

```
## # A tibble: 252 x 1
##    year_acquired
##            <dbl>
##  1          1937
##  2          1938
##  3          1940
##  4          1941
##  5          1941
##  6          1942
##  7          1942
##  8          1942
##  9          1942
## 10          1943
## # … with 242 more rows
```

```r
# This worked but it doesn't show any other data.
```



```r
# This also worked
moma %>% 
  select(title, artist, year_acquired, year_created, artist_gender) %>%
  filter(artist_gender == "Female") %>%
  arrange(year_acquired)
```

```
## # A tibble: 252 x 5
##    title                artist          year_acquired year_created artist_gender
##    <chr>                <chr>                   <dbl>        <dbl> <chr>        
##  1 Landscape, 47        Natalia Goncha…          1937         1912 Female       
##  2 Shack                Loren MacIver            1938         1934 Female       
##  3 Hopscotch            Loren MacIver            1940         1940 Female       
##  4 Shadows with Painti… Irene Rice Per…          1941         1940 Female       
##  5 Figure               Varvara Stepan…          1941         1921 Female       
##  6 Still Life in Red    Amelia Peláez …          1942         1938 Female       
##  7 White Lines          Irene Rice Per…          1942         1942 Female       
##  8 Musical Squash       Maud Morgan              1942         1942 Female       
##  9 Desolation           Raquel Forner            1942         1942 Female       
## 10 Self-Portrait with … Frida Kahlo              1943         1940 Female       
## # … with 242 more rows
```

```r
moma %>% 
  filter(num_artists == 1 & n_female_artists == 1) %>% 
  select(title, artist, year_acquired, year_created) %>% 
  arrange(year_acquired)
```

```
## # A tibble: 252 x 4
##    title                        artist                year_acquired year_created
##    <chr>                        <chr>                         <dbl>        <dbl>
##  1 Landscape, 47                Natalia Goncharova             1937         1912
##  2 Shack                        Loren MacIver                  1938         1934
##  3 Hopscotch                    Loren MacIver                  1940         1940
##  4 Shadows with Painting        Irene Rice Pereira             1941         1940
##  5 Figure                       Varvara Stepanova              1941         1921
##  6 Still Life in Red            Amelia Peláez Del Ca…          1942         1938
##  7 White Lines                  Irene Rice Pereira             1942         1942
##  8 Musical Squash               Maud Morgan                    1942         1942
##  9 Desolation                   Raquel Forner                  1942         1942
## 10 Self-Portrait with Cropped … Frida Kahlo                    1943         1940
## # … with 242 more rows
```

## What is the oldest painting bhye a solo female artist, and when was it created?


```r
moma %>% filter(num_artists == 1 & n_female_artists == 1) %>% 
  select(title, artist, year_acquired, year_created) %>% 
  arrange(year_created)
```

```
## # A tibble: 252 x 4
##    title                              artist          year_acquired year_created
##    <chr>                              <chr>                   <dbl>        <dbl>
##  1 Self-Portrait with Two Flowers in… Paula Modersoh…          2017         1907
##  2 Girl with Bare Shoulders           Gwen John                1958         1909
##  3 Girl Reading at a Window           Gwen John                1971         1911
##  4 Landscape, 47                      Natalia Goncha…          1937         1912
##  5 Cubist Nude                        Alexandra Exter          1991         1912
##  6 Rayonism, Blue-Green Forest        Natalia Goncha…          1985         1913
##  7 The Factory and the Bridge         Olga Rozanova            1985         1913
##  8 Subject from a Dyer's Shop         Lyubov Popova            1985         1914
##  9 Portuguese Market                  Sonia Delaunay…          1955         1915
## 10 Girl with a Blue Scarf             Gwen John                1963         1915
## # … with 242 more rows
```

# Basic Plotting!

## Year painted vs. year acquired


```r
ggplot(moma, aes(x = year_created, y = year_acquired)) +
  geom_point(alpha = 0.2, na.rm = TRUE) +
  geom_abline(intercept = c(0,0), color = "Red") +
  labs(x = "Year Painted", y = "Year Acquired") +
  ggtitle("MoMA Keeps Its Collection Current")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

## Faceting by gender

```r
moma_solo <- moma %>% 
  filter(num_artists == 1)
ggplot(moma_solo, aes(x = year_created, y = year_acquired)) +
  geom_point(alpha = .1) +
  geom_abline(intercept = c(0,0), color = "Red") +
  labs(x = "Year Painted", y = "Year Acquired") +
  ggtitle("MoMa Keeps Its Collection Current") +
  facet_wrap(~artist_gender)
```

```
## Warning: Removed 14 rows containing missing values (geom_point).
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="672" />

# Exploring Painting Dimensions

## Challenge #4

```r
ggplot(moma, aes(x = width_cm, y = height_cm)) +
  geom_point() +
  labs(x = "Width", y = "Height") +
  ggtitle("MoMA Paintings, Tall and Wide")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-17-1.png" width="672" />


```r
moma_dim <- moma %>% 
  filter(height_cm < 600, width_cm < 760) %>% 
  mutate(hw_ratio = height_cm / width_cm,
         hw_cat = case_when(
           hw_ratio > 1 ~ "taller than wide",
           hw_ratio < 1 ~ "wider than tall",
           hw_ratio == 1 ~ "perfect square"
         ))
library(ggthemes)
ggplot(moma_dim, aes(x = width_cm, y = height_cm, colour = hw_cat)) +
  geom_point(alpha = .5) +
  ggtitle("MoMA Paintings, Tall and Wide") +
  scale_colour_manual(name = "",
                      values = c("gray50", "#FF9900", "#B14CF0")) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) +
  labs(x = "Width", y = "Height")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />

## Different colors


## Experimenting with `geom_annotate()`

```r
#library(ggthemes)

ggplot(moma_dim, aes(x = width_cm, y = height_cm, colour = hw_cat)) +
  geom_point(alpha = .5, show.legend = FALSE) +
  ggtitle("MoMA Paintings, Tall and Wide") +
  scale_colour_manual(name = "",
                      values = c("gray50", "#ee5863", "#6999cd")) +
  theme_fivethirtyeight() +
  theme(axis.title = element_text()) +
  labs(x = "Width", y = "Height") +
  annotate(x = 200, y = 400, geom = "text", 
           label = "Taller than\nWide", color = "#ee5863", 
           size = 5, family = "Palatino", hjust =1, fontface = 2) +
    annotate(x = 375, y = 100, geom = "text", 
             label = "Wider than\nTall", color = "#6999cd", 
             size = 5, family = "Palatino", hjust = 0, fontface = 2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-20-1.png" width="672" />

# Challenge #5, on your own!




```r
library(ggthemes)
v <- ggplot(moma_dim, aes(x= hw_cat, y = year_created)) +
  geom_violin() +
  labs(x = "Shape of Painting", y = "Year Created") +
  ggtitle("MoMA Paintings Shape Trends from 1872-2017")
v
```

```
## Warning: Removed 5 rows containing non-finite values (stat_ydensity).
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-22-1.png" width="672" />


```r
box <- v + geom_boxplot(width = 0.1)
box
```

```
## Warning: Removed 5 rows containing non-finite values (stat_ydensity).
```

```
## Warning: Removed 5 rows containing non-finite values (stat_boxplot).
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-23-1.png" width="672" />

```r
color_v <- ggplot(moma_dim, aes(x= hw_cat, y = year_created)) +
  geom_violin(trim = FALSE, fill = '#A4A4A4', color = "darkred") +
  labs(x = "Shape of Painting", y = "Year Created") +
  ggtitle("MoMA Paintings Shape Trends from 1872-2017")
color_v
```

```
## Warning: Removed 5 rows containing non-finite values (stat_ydensity).
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-24-1.png" width="672" />

```r
v + scale_fill_brewer(palette="Blues")
```

```
## Warning: Removed 5 rows containing non-finite values (stat_ydensity).
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-25-1.png" width="672" />


```r
#grid.arrange(v,box, color_v, ncol = 3)
```


I wanted to see if there were trends in dimensions of the MoMA paintings. If there any sort of cycle of paintings that were a perfect square size? Every 30 years? What about the other sizes? My initial thought was to make a continuous data graph of sizes over time, maybe a geom_point and geom_line to model the data. But it thought it might look better as a geom_violin plot where there are three categories on the x-axis and years_created on the y-axis.
