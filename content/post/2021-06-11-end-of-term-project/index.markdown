---
title: End of term project
author: Christopher P. Loo
date: '2021-06-11'
slug: end-of-term-project
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-06-11T23:56:31-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---


## HLA status from patients with AML

Data is publically available from OHSU.
www.vizome.org






```r
library(tidyverse)
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

```r
library(ggvis)
```

```
## 
## Attaching package: 'ggvis'
```

```
## The following object is masked from 'package:ggplot2':
## 
##     resolution
```

```r
my_data <- read.csv(file = "/Users/looc/Box/2021_Spring_classes/Data_Viz/blogdown/Chris_Loo_website/content/post/2021-06-11-end-of-term-project/BeatAML_HLA.csv", header=TRUE)
my_data <- as_tibble(my_data)
my_data
```

```
## # A tibble: 1,339 x 16
##    SeqID  HLA_A_1 HLA_A_2 HLA_B_1 HLA_B_2 HLA_C_1 HLA_C_2 PatientID SpecimenType
##    <chr>  <chr>   <chr>   <chr>   <chr>   <chr>   <chr>       <int> <chr>       
##  1 30-00… A*11:01 A*23:01 B*35:01 B*44:03 C*04:01 C*04:01      4782 Peripheral …
##  2 30-00… A*11:01 A*23:01 B*35:01 B*44:03 C*04:01 C*04:01      4782 Skin Biopsy 
##  3 30-00… A*02:01 A*02:01 B*18:01 B*15:01 C*04:01 C*05:01      4770 Bone Marrow…
##  4 30-00… A*02:01 A*02:01 B*18:01 B*15:01 C*04:01 C*05:01      4770 Skin Biopsy 
##  5 30-00… A*02:01 A*02:01 B*35:03 B*40:01 C*03:04 C*04:01      4765 Peripheral …
##  6 30-00… A*02:01 A*02:01 B*35:03 B*40:01 C*03:04 C*04:01      4765 Skin Biopsy 
##  7 30-00… A*01:01 A*33:03 B*08:01 B*50:01 C*07:01 C*06:02      4762 Bone Marrow…
##  8 30-00… A*01:01 A*33:03 B*08:01 B*50:01 C*07:01 C*06:02      4762 Skin Biopsy 
##  9 30-00… A*30:02 A*02:01 B*56:01 B*35:12 C*04:01 C*01:02      4736 Bone Marrow…
## 10 30-00… A*02:01 A*30:02 B*56:01 B*35:12 C*04:01 C*01:02      4736 Skin Biopsy 
## # … with 1,329 more rows, and 7 more variables: Patient. <int>, Errors <chr>,
## #   Original_LabID <chr>, PatientID.1 <int>, Total.patients <int>,
## #   X.Mismatch <chr>, Total_matched. <chr>
```


```{recho=FALSE}
#Reverse sort
my_data <- my_data[order(my_data$PatientID, decreasing = FALSE),]
#my_data
```


```r
#Keep only first duplicate of my_data$PatientID
unique_patID <- my_data[!duplicated(my_data$PatientID),]
#unique_patID
```



```r
HLA_A_stack <- stack(unique_patID, select = c(HLA_A_1, HLA_A_2))
HLA_B_stack <- stack(unique_patID, select = c(HLA_B_1, HLA_B_2))
HLA_C_stack <- stack(unique_patID, select = c(HLA_C_1, HLA_C_2))
num_HLA_A <-  count(HLA_A_stack, values)
num_HLA_B <-  count(HLA_B_stack, values)
num_HLA_C <-  count(HLA_C_stack, values)
num_HLA_A <- num_HLA_A[order(num_HLA_A$n, decreasing = TRUE),]
num_HLA_B <- num_HLA_B[order(num_HLA_B$n, decreasing = TRUE),]
num_HLA_C <- num_HLA_C[order(num_HLA_C$n, decreasing = TRUE),]
num_HLA_A
```

```
##     values   n
## 2  A*02:01 358
## 1  A*01:01 202
## 15 A*03:01 188
## 20 A*24:02 131
## 17 A*11:01  89
## 27 A*29:02  50
## 35 A*32:01  48
## 42 A*68:01  41
## 25 A*26:01  39
## 34 A*31:01  38
## 49    <NA>  36
## 24 A*25:01  35
## 19 A*23:01  26
## 36 A*33:01  20
## 37 A*33:03  15
## 4  A*02:05  14
## 29 A*30:01  14
## 43 A*68:02  14
## 30 A*30:02  13
## 5  A*02:06  12
## 41 A*66:01   9
## 39 A*34:02   8
## 3  A*02:02   6
## 7  A*02:11   4
## 21 A*24:03   4
## 6  A*02:07   3
## 12 A*02:74   3
## 16 A*03:02   3
## 22 A*24:07   3
## 10 A*02:17   2
## 26 A*29:01   2
## 32 A*30:09   2
## 38 A*34:01   2
## 40 A*36:01   2
## 46 A*69:01   2
## 47 A*74:01   2
## 48 A*74:02   2
## 8  A*02:13   1
## 9  A*02:16   1
## 11 A*02:24   1
## 13 A*02:90   1
## 14 A*02:93   1
## 18 A*11:04   1
## 23 A*24:10   1
## 28 A*29:10   1
## 31 A*30:04   1
## 33 A*30:11   1
## 44 A*68:03   1
## 45 A*68:05   1
```



```r
library(ggplot2)
library(ggpubr)
theme_set(theme_pubr())
ggplot(num_HLA_A, aes(x = values, y = n)) +
  geom_bar(fill = "#00AFBB", stat = "identity") +
  geom_text(aes(label = n), vjust = -0.3) + ggtitle("Population Frequency of HLA-A") +
  theme_pubclean() + theme(axis.text.x = element_text(angle = 90)) + xlab("HLA A") + ylab("Counts")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />


```r
ggplot(num_HLA_B, aes(x = values, y = n)) +
  geom_bar(fill = "#FC4E07", stat = "identity") +
  geom_text(aes(label = n), vjust = -0.3) + ggtitle("Population Frequency of HLA-B") +
  theme_pubclean() + theme(axis.text.x = element_text(angle = 90)) + xlab("HLA B") + ylab("Counts")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />


```r
ggplot(num_HLA_C, aes(x = values, y = n)) +
  geom_bar(fill = "#E7B800", stat = "identity") +
  geom_text(aes(label = n), vjust = -0.3) + ggtitle("Population Frequency of HLA-C") +
  theme_pubclean() + theme(axis.text.x = element_text(angle = 90)) + xlab("HLA C") + ylab("Counts")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />








```r
library(dplyr)
HLA_A1_agg <- count(unique_patID, HLA_A_1)
HLA_A1_agg[order(HLA_A1_agg$n, decreasing = TRUE),]
```

```
## # A tibble: 38 x 2
##    HLA_A_1     n
##    <chr>   <int>
##  1 A*02:01   255
##  2 A*01:01   103
##  3 A*24:02    73
##  4 A*03:01    36
##  5 A*29:02    36
##  6 A*31:01    33
##  7 A*11:01    23
##  8 A*32:01    22
##  9 <NA>       18
## 10 A*26:01    14
## # … with 28 more rows
```




```r
HLA_A2_agg <- count(unique_patID,HLA_A_2)
HLA_A2_agg[order(HLA_A2_agg$n, decreasing = TRUE),]
```

```
## # A tibble: 42 x 2
##    HLA_A_2     n
##    <chr>   <int>
##  1 A*03:01   152
##  2 A*02:01   103
##  3 A*01:01    99
##  4 A*11:01    66
##  5 A*24:02    58
##  6 A*68:01    34
##  7 A*32:01    26
##  8 A*26:01    25
##  9 A*25:01    24
## 10 A*23:01    18
## # … with 32 more rows
```


```r
HLA_B1_agg <- count(unique_patID,HLA_B_1)
HLA_B1_agg[order(HLA_B1_agg$n, decreasing = TRUE),]
```

```
## # A tibble: 59 x 2
##    HLA_B_1     n
##    <chr>   <int>
##  1 B*08:01   101
##  2 B*07:02    90
##  3 B*44:02    77
##  4 B*44:03    58
##  5 B*35:01    47
##  6 B*18:01    37
##  7 B*57:01    37
##  8 B*51:01    22
##  9 B*27:05    21
## 10 B*14:02    19
## # … with 49 more rows
```


```r
HLA_B2_agg <- count(unique_patID,HLA_B_2)
HLA_B2_agg[order(HLA_B2_agg$n, decreasing = TRUE),]
```

```
## # A tibble: 81 x 2
##    HLA_B_2     n
##    <chr>   <int>
##  1 B*15:01    75
##  2 B*07:02    57
##  3 B*51:01    48
##  4 B*40:01    43
##  5 B*08:01    31
##  6 B*27:05    31
##  7 B*35:01    31
##  8 B*44:02    31
##  9 B*18:01    29
## 10 B*14:02    28
## # … with 71 more rows
```


```r
HLA_C1_agg <- count(unique_patID,HLA_C_1)
HLA_C1_agg[order(HLA_C1_agg$n, decreasing = TRUE),]
```

```
## # A tibble: 29 x 2
##    HLA_C_1     n
##    <chr>   <int>
##  1 C*07:02   155
##  2 C*07:01    99
##  3 C*03:04    76
##  4 C*03:03    73
##  5 C*05:01    57
##  6 C*04:01    48
##  7 C*06:02    37
##  8 C*02:02    30
##  9 C*12:03    30
## 10 C*01:02    25
## # … with 19 more rows
```


```r
HLA_C2_agg <- count(unique_patID,HLA_C_2)
HLA_C2_agg[order(HLA_C2_agg$n, decreasing = TRUE),]
```

```
## # A tibble: 32 x 2
##    HLA_C_2     n
##    <chr>   <int>
##  1 C*04:01   101
##  2 C*07:01    95
##  3 C*06:02    88
##  4 C*08:02    52
##  5 C*12:03    50
##  6 C*05:01    49
##  7 C*16:01    40
##  8 C*01:02    39
##  9 C*07:02    34
## 10 C*15:02    31
## # … with 22 more rows
```



```r
library(forcats)
ggplot(mutate(unique_patID, HLA_A_1 = fct_infreq(HLA_A_1))) + geom_bar(aes(x = HLA_A_1)) +
  theme(axis.text.x=element_text(angle=90,hjust=1,vjust=0.5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />




```r
A1 <- unique_patID$HLA_A_1
A2 <-  unique_patID$HLA_A_2
patID <- unique_patID$PatientID
combined_HLA_A <- c(A1,A2)
HLA_A <- as_tibble(combined_HLA_A)
#HLA_A <- unique(HLA_A)
table(HLA_A)
```

```
## HLA_A
## A*01:01 A*02:01 A*02:02 A*02:05 A*02:06 A*02:07 A*02:11 A*02:13 A*02:16 A*02:17 
##     202     358       6      14      12       3       4       1       1       2 
## A*02:24 A*02:74 A*02:90 A*02:93 A*03:01 A*03:02 A*11:01 A*11:04 A*23:01 A*24:02 
##       1       3       1       1     188       3      89       1      26     131 
## A*24:03 A*24:07 A*24:10 A*25:01 A*26:01 A*29:01 A*29:02 A*29:10 A*30:01 A*30:02 
##       4       3       1      35      39       2      50       1      14      13 
## A*30:04 A*30:09 A*30:11 A*31:01 A*32:01 A*33:01 A*33:03 A*34:01 A*34:02 A*36:01 
##       1       2       1      38      48      20      15       2       8       2 
## A*66:01 A*68:01 A*68:02 A*68:03 A*68:05 A*69:01 A*74:01 A*74:02 
##       9      41      14       1       1       2       2       2
```





```r
z <- count(HLA_A, value)
z
```

```
## # A tibble: 49 x 2
##    value       n
##    <chr>   <int>
##  1 A*01:01   202
##  2 A*02:01   358
##  3 A*02:02     6
##  4 A*02:05    14
##  5 A*02:06    12
##  6 A*02:07     3
##  7 A*02:11     4
##  8 A*02:13     1
##  9 A*02:16     1
## 10 A*02:17     2
## # … with 39 more rows
```


```r
library(forcats)
library(ggthemes)
plot_A <- ggplot(mutate(HLA_A, value = fct_infreq(value))) + geom_bar(aes(x = value, fill = "orange")) + 
  labs(x = "HLA-A type") + theme(axis.text.x=element_text(angle = 90,hjust = 1,vjust = 0.5)) + ggtitle("Population Frequency of HLA-A") + theme(legend.position = "none")
A <- plot_A + 
  theme(plot.title = element_text(hjust = 0.5)) +
  theme(axis.text = element_text(size = 10)) +
  theme(panel.background = element_blank()) +
  theme(axis.line.x = element_line(color = "gray80", size = 0.5)) +
  theme(axis.ticks = element_line(color = "gray80", size = 0.5))
A
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

```r
ggsave("HLA_A.png", A )
```

```
## Saving 7 x 5 in image
```


```r
B1 <- unique_patID$HLA_B_1
B2 <-  unique_patID$HLA_B_2
patID <- unique_patID$PatientID
combined_HLA_B <- c(B1,B2)
HLA_B <- as_tibble(combined_HLA_B)
#HLA_A <- unique(HLA_A)
table(HLA_B)
```

```
## HLA_B
## B*07:02 B*07:04 B*07:05 B*07:36 B*08:01 B*08:23 B*13:01 B*13:02 B*14:01 B*14:02 
##     147       1       3       1     132       1       3      24      12      47 
## B*14:03 B*15:01 B*15:02 B*15:03 B*15:07 B*15:08 B*15:10 B*15:15 B*15:16 B*15:17 
##       1      89       2       3       3       1       5       2       2       5 
## B*15:18 B*15:24 B*15:25 B*15:30 B*15:35 B*15:39 B*18:01 B*18:02 B*18:04 B*27:02 
##       3       1       1       2       2       1      66       2       1       4 
## B*27:05 B*27:08 B*35:01 B*35:02 B*35:03 B*35:05 B*35:08 B*35:12 B*35:17 B*35:24 
##      52       1      78      10      19       2       6       4       3       1 
## B*35:34 B*37:01 B*38:01 B*38:02 B*39:01 B*39:02 B*39:05 B*39:06 B*39:08 B*40:01 
##       1      12      29       1      22       1       7       9       1      59 
## B*40:02 B*40:05 B*40:06 B*40:08 B*41:01 B*41:02 B*42:01 B*44:02 B*44:03 B*44:04 
##      27       1       3       1       5       5       6     108      71       1 
## B*44:05 B*44:06 B*44:13 B*45:01 B*46:01 B*47:01 B*48:01 B*48:08 B*49:01 B*50:01 
##       4       1       1       9       6       2       8       1      19      18 
## B*50:02 B*51:01 B*51:02 B*51:07 B*51:08 B*52:01 B*53:01 B*54:01 B*55:01 B*55:02 
##       2      70       2       1       1      18      12       1      28       1 
## B*56:01 B*57:01 B*57:02 B*57:03 B*58:01 B*58:02 B*67:01 B*78:01 B*81:01 
##      14      60       3       3       9       4       2       1       4
```



```r
plot_B <- ggplot(mutate(HLA_B, value = fct_infreq(value))) + geom_bar(aes(x = value)) +
  theme(axis.text.x=element_text(angle=90,hjust=1,vjust= 0.5)) 
plot_B + ggtitle("Population Frequency of HLA-B") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />



```r
library(ggplot2)
library(ggpubr)
theme_set(theme_pubr())
ggplot(num_HLA_A, aes(x = values, y = n)) +
  geom_bar(fill = "#0073C2FF", stat = "identity") +
  geom_text(aes(label = n), vjust = -0.3) + ggtitle("Population Frequency of HLA-A") +
  theme_pubclean() + theme(axis.text.x = element_text(angle = 90))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="672" />



```r
C1 <- unique_patID$HLA_C_1
C2 <-  unique_patID$HLA_C_2
patID <- unique_patID$PatientID
combined_HLA_C <- c(C1,C2)
HLA_C <- as_tibble(combined_HLA_C)
#HLA_A <- unique(HLA_A)
table(HLA_C)
```

```
## HLA_C
## C*01:02 C*01:08 C*02:02 C*02:10 C*03:02 C*03:03 C*03:04 C*03:05 C*03:06 C*04:01 
##      64       1      60       3       7      88     100       2       2     149 
## C*04:03 C*05:01 C*06:02 C*07:01 C*07:02 C*07:04 C*07:06 C*07:46 C*07:95 C*08:01 
##       1     106     125     194     189      18       2       1       1      12 
## C*08:02 C*08:03 C*08:04 C*08:15 C*12:02 C*12:03 C*14:02 C*14:04 C*15:02 C*15:04 
##      57       1       2       1      15      80      20       1      36       1 
## C*15:05 C*16:01 C*16:02 C*16:04 C*17:01 C*18:01 
##       2      56       1       1      15       4
```



```r
library(forcats)
plot_C <- ggplot(mutate(HLA_C, value = fct_infreq(value))) + geom_bar(aes(x = value)) +
  theme(axis.text.x=element_text(angle=90,hjust=1,vjust=0.5))
plot_C + ggtitle("Population Frequency of HLA-C") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-21-1.png" width="672" />

