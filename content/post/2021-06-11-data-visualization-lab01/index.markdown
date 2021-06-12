---
title: Data Visualization Lab01
author: Christopher P. Loo
date: '2021-06-11'
slug: data-visualization-lab01
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-06-11T19:17:26-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---






# Workflow for Lab 01

The lab instructions can be found [here](https://stevenbedrick.github.io/data-vis-labs-2021/01-eda_hot_dogs.html); we will work through its contents together via Webex. You will use this RMarkdown file as your workspace and final document. Don't forget to update the "author" metadata field at the top of the file!

# Start by Loading Libraries



# Loading and wrangling data

## Loading

_Tip_: The data files for this lab are already loaded into this project, under the `data/` directory.


```r
hot_dogs <- read_csv(here::here("content", "post","2021-06-11-data-visualization-lab01","data","hot_dog_contest.csv"), 
    col_types = cols(
      gender = col_factor(levels = NULL)
    ))
```

**Why is better to import data with the extra line of of - col_types - than without it by just using hot_dogs <- read.csv("data/hot_dog_contest.csv")?**

**The difference that I notice is that the gender column is without quotes. Does changing the gender column to a factor make it available to count the number of 'male' and 'female' contestants?**



```r
glimpse(hot_dogs)
```

```
Error in glimpse(hot_dogs): object 'hot_dogs' not found
```


```r
hot_dogs
```

```
Error in eval(expr, envir, enclos): object 'hot_dogs' not found
```


** I have used skimr in the past and for some data it is really helpful to get small histograms of the data.**


```r
library(skimr)
skim(hot_dogs)
```

```
Error in skim(hot_dogs): object 'hot_dogs' not found
```

## Wrangling

Filtering and indicator field


```r
## mutate w/o the >= 1997 will produce the same information as the first column (year.) Here a logical of TRUE or FALSE will result.
 
hot_dogs <- hot_dogs %>% 
  mutate(post_ifoce = year >= 1997) %>%   
  filter(year >= 1981 & gender == 'male')  ## ampersand (&) must be used here not "and"
```

```
Error in mutate(., post_ifoce = year >= 1997): object 'hot_dogs' not found
```

```r
hot_dogs
```

```
Error in eval(expr, envir, enclos): object 'hot_dogs' not found
```

# Initial Plot


```r
ggplot(hot_dogs, aes(x= year, y= num_eaten)) +
  geom_col()
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```

** I don't mind this boring looking plot but it would be nice to have a title and more year labels, every 2 or 5 years.**

# Axis Labels & Title


```r
ggplot(hot_dogs, aes(x = year, y = num_eaten)) +
  geom_col() +
  labs(x = "Year", y = "Hot Dogs and Buns Consumed") +
  ggtitle("Nathan's Hot Dog Eating Contest Results, 1981-2017")
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```

# Initial Colors & Styling

## Challenge 1: 3 Different Versions


```r
# White outlines version
ggplot(hot_dogs, aes(x = year, y = num_eaten)) +
  geom_col(colour = "white")
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```


```r
# White outlines and blue columns version

ggplot(hot_dogs, aes(x = year, y = num_eaten)) +
  geom_col(colour = "white", fill = "navyblue") +  
  labs(x = "Year", y = "Hot dogs and Buns Consumed") +
  ggtitle("Nathan's Hot Dog Eating Contest Results, 1981-2017")
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```


```r
# White outlines version
# can use aes not only for mapping but for defining the aesthetics of geom_col

ggplot(hot_dogs, aes(x = year, y = num_eaten)) +
  geom_col(aes(fill = post_ifoce), colour = "white") +  
  labs(x = "Year", y = "Hot dogs and Buns Consumed") +
  ggtitle("Nathan's Hot Dog Eating Contest Results, 1981-2017") 
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```
## Challenge 2: Adjusting the Legend


```r
# Deleted legend
ggplot(hot_dogs, aes(x = year, y = num_eaten)) +
  geom_col(aes(fill = post_ifoce), colour = "white") +
  labs(x = "Year", y = "Hot dogs and Buns Consumed") +
  ggtitle("Nathans Hot Dog Earting Contest Results, 1981-2017") +
  theme(legend.position = "none")
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```


```r
ggplot(hot_dogs, aes(x = year, y = num_eaten)) +
  geom_col(aes(fill = post_ifoce), colour = "white") +  
  labs(x = "Year", y = "Hot dogs and Buns Consumed") +
  ggtitle("Nathan's Hot Dog Eating Contest Results, 1981-2017") +
  scale_fill_discrete(name = "", labels = c("Pre-IFOCE", "Post-IFOCE"))
```

```
Error in ggplot(hot_dogs, aes(x = year, y = num_eaten)): object 'hot_dogs' not found
```

# Change the Dataset 

IFOCE affiliation analysis

## Challenge 3: Adding Columns, Filtering


```r
hot_dogs_af <- read_csv(here::here("content", "post","2021-06-11-data-visualization-lab01","data", "hot_dog_contest_with_affiliation.csv"), 
    col_types = cols(
      affiliated = col_factor(levels = NULL), 
      gender = col_factor(levels = NULL)
      )) %>% 
  mutate(post_ifoce = year >= 1997) %>% 
  filter(year >= 1981 & gender == "male")
```


```r
glimpse(hot_dogs_af)
```

```
Error in glimpse(hot_dogs_af): object 'hot_dogs_af' not found
```

## Challenge 4: EDA


```r
# Using dplyer::distinct to figure out how many unique values there are of affiliated
hot_dogs_af %>% distinct(affiliated)
```

```
Error in distinct(., affiliated): object 'hot_dogs_af' not found
```


```r
# Using dplyr::count to count the number of rows for each unique value of affiliated
hot_dogs_af %>% count(affiliated)
```

```
Error in count(., affiliated): object 'hot_dogs_af' not found
```

## Updated Plot


```r
# Making a plot identifying values that are affiliated

ggplot(hot_dogs_af, aes(x = year, y = num_eaten)) +
  geom_col(aes(fill= affiliated)) +
  labs(x = "Year", y = "Hot dogs and Buns Consumed") +
  ggtitle("Nathan's Hot Dog Eating Contest Results, 1981-2017")
```

```
Error in ggplot(hot_dogs_af, aes(x = year, y = num_eaten)): object 'hot_dogs_af' not found
```

## Challenge 5: Adjusting the Colors


```r
# Updating colours of the plot

affil_plot <- ggplot(hot_dogs_af, aes(x = year, y = num_eaten)) +
  geom_col(aes(fill = affiliated)) +
  labs(x = "Year", y = "Hot Dogs and Buns Consumed") +
  ggtitle("Nathan's Hot Dog Eating Contest Results, 1981-2017") +
  scale_fill_manual(name = "IFOC-affiliation", values = c('#E9602B','#2277A0','#CCB683'))
```

```
Error in ggplot(hot_dogs_af, aes(x = year, y = num_eaten)): object 'hot_dogs_af' not found
```

```r
affil_plot
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```

# Sweating the Details

## Initial plot


```r
# Adjusting the scales: default scales are c(0.05, 0) for continuous variables. The first number is multiplicative and second is additive. This brings the colored bars resting on both the x and y axis.

affil_plot <- affil_plot + 
  scale_y_continuous(expand = c(0, 0),
                     breaks = seq(0, 80, 10)) +
  scale_x_continuous(expand = c(0, 0))
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```

```r
affil_plot
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```

**Personally, I like having a little bit of space between the top of the orange bars. I increased the y breaks to equal seq(0, 80, 10) but nothing changed.**


## Challenge 6: Custom Axes


```r
affil_plot <- affil_plot + 
  coord_cartesian(xlim = c(1980, 2018), ylim = c(0, 80))
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```

```r
affil_plot
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```

# Themes

## Initial Plot


```r
# Customizing the placement of the title, axis font size, removing the background color, and adjusting the color of the axis and ticks
# default hjust: 0 == left, 0.5 == centered, 1 == right

affil_plot + 
  theme(plot.title = element_text(hjust = 0.5)) +
  theme(axis.text = element_text(size = 10)) +
  theme(panel.background = element_blank()) +
  theme(axis.line.x = element_line(color = "gray80", size = 0.5)) +
  theme(axis.ticks = element_line(color = "gray80", size = 0.5))
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```

**I think without the background panel the plot looks better but is harder to interpret.**

## Making a Custom Theme


```r
# Might be easier to use a theme by creating a variable representing all of the settings

hot_diggity <- theme(plot.title = element_text(hjust = 0.5),
                     axis.text = element_text(size = 10),
                     panel.background = element_blank(),
                     axis.line.x = element_line(color = "gray80", size = 0.5),
                     axis.ticks = element_line(color = "gray80", size = 0.5),
                     text = element_text(family = "Lato") # need extrafont for this
                     )
```

`





```r
# Another library theme

affil_plot + theme_tufte(base_family = "Palatino")
```

```
Error in eval(expr, envir, enclos): object 'affil_plot' not found
```






