---
title: Data Visualization Lab04
author: Christopher P. Loo
date: '2021-06-11'
slug: data-visualization-lab04
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-06-11T23:47:05-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

### Graph discription -- Chord Diagram 
A chord diagram is a circular plot that shows the relationships between entities and details of those relationships. All of the entities are distributed on and around the surface of a circle, and the entities only occupy a continuous portion of the total circumference. Lines or "chords" stretch across the circle to connect and show relationships between two entities. The connecting line between to entities shows not only a relationship but quantification of the relationship. Chord diagrams are often colored to help visualize relationships between all of the entities.  


### DATA
This data set was taken from the larger data set located at:   https://github.com/davestroud/BeerStudy/blob/master/Beers.csv  

The data a listing of types of beers, their alcohol content (ABV), bitterness/hoppiness (IBU), and the ounces typically served.  
I did not use the whole data set because in this example I thought it would be easier to understand the details of the chord diagram with fewer entities and having the lines/chords across the circle to have clear differences from each other. However, one can infer that having too many entities is a downfall to chord diagrams.  

### Representation Description  

What I am trying to show is the various types of beer categories have different various levels of alcohol content, better or hoppy complexity, and typical volumes of how the particular beer is served from other types of beers. This is often detailed in a table but looking at a list of numbers makes it hard to compare or find trends in data.   

The parts of a chord diagram starts with a circular shape.   
1. Node: is a continuous portion of the circumference of the circle which represents a category of the data that will will be one entity of the relationship.  
  + The length of the node can represent the scale of the category it is representing.  
2. Arc: is the "chord" or line that connects two nodes to each other.  
  +The width of the chord represents the strength of the relationship of the two entities.  
3. Color: Color is used to distinguish chords and nodes from each other.  


### How to read and what to look for:  
Chord diagrams can be very simple or describe a very complex visualization of many different factors of the entities themselves or about the relationships they possess with other entities. However, chord diagrams can give a good initial view of common relationships between entities.  

What to look for when analyzing chord diagrams is the proportion node lengths for each entity, the number of arcs going to or from each entity, the width of each line in comparison to others, as well as the width of a line at one entity space on the circle and the width at the associated entity space. Often, colors of the lines do not represent anything but to represent a differentiation between entities.  

If one entity has many lines coming to or from its space on the circle it shows that entity has many relationships within the dataset.  

Furthermore, if an arc is very thick or wide it could mean that the relationship between those two entities is very strong, perhaps highly correlated.  

The data I am representing by a chord diagram will show row names and column names of a matrix of the data. The lines will represent the value of a point in the matrix.  

### Presentation Tips  
The presentation tips for chord diagrams is listed below:  
**1. Annotation** and supplementary information can be very useful with annotations, especially with providing quantitative data that is not presented directly on the chord diagram. It also, can help emphasize and summarize the important areas of the diagram where the creator/author wants the user to understand.  
**2. Color** is very useful in making chord diagrams. The diagram is likely to many arcs and to distinguish each arc from each other, colors are used.  
**3. Size of the plot** plays also plays a role in chord diagrams. Every measured in a chord diagram is going to need a label and if there is a massive number of arc it will be hard to follow individual arcs.  
  
### Variations and alternatives  
There is a variation of the chord diagram named **the ribbon diagram.** In the ribbon diagram there are equally spaced nodes on a circle and arc spanning to respective nodes. This diagram focuses on more on the connections and less of the quantification of nodes and arcs.  
  
  


```r
# Install package
#install.packages("circlize")
library(circlize)
```

```
## ========================================
## circlize version 0.4.13
## CRAN page: https://cran.r-project.org/package=circlize
## Github page: https://github.com/jokergoo/circlize
## Documentation: https://jokergoo.github.io/circlize_book/book/
## 
## If you use it in published research, please cite:
## Gu, Z. circlize implements and enhances circular visualization
##   in R. Bioinformatics 2014.
## 
## This message can be suppressed by:
##   suppressPackageStartupMessages(library(circlize))
## ========================================
```


### Load Data 
 

### chord diagram is can be a good representation of the associations of different variables. 


```r
#Load data
numbers <- c(0.055,0.07,0.085,0.099,0.082,17,80,11,92,103,16,12,12,8.4,12)

#Creating Matrix
beer_matrix <- matrix(numbers, nrow=5,ncol=3,byrow=FALSE)

#Labeling rows and columns
rownames(beer_matrix) <- c("Pilsner", "IPA", "Stout", "Barley_Wine", "Double_IPA")
colnames(beer_matrix) <- c("ABV ", "IBU", "Ounces")

#Print beer_matrix
beer_matrix
```

```
##              ABV  IBU Ounces
## Pilsner     0.055  17   16.0
## IPA         0.070  80   12.0
## Stout       0.085  11   12.0
## Barley_Wine 0.099  92    8.4
## Double_IPA  0.082 103   12.0
```

#### Defining category colors


```r
col.pal <- c(Pilsner = "green", IPA = "red", Stout = "blue", Barley_Wine = "grey", Double_IPA = "maroon",
             ABV = "grey", IBU = "black", Ounces = "grey")
```

### Making Chord Diagram

```r
chordDiagram(beer_matrix, grid.col = col.pal)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />
