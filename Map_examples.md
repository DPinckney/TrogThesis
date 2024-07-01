Untitled
================

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax
for authoring HTML, PDF, and MS Word documents. For more details on
using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that
includes both content as well as the output of any embedded R code
chunks within the document. You can embed an R code chunk like this:

``` r
my_species <-read.csv("pettalidae_maps.csv")


library("ggplot2")
library("sf")
```

    ## Warning: package 'sf' was built under R version 4.1.2

    ## Linking to GEOS 3.9.1, GDAL 3.4.0, PROJ 8.1.1; sf_use_s2() is TRUE

``` r
library("rnaturalearth")
library("rnaturalearthdata")
library("ggspatial")

theme_set(theme_bw())

world <- ne_countries(scale = "medium", returnclass = "sf")
class(world)
```

    ## [1] "sf"         "data.frame"

``` r
map1 <- ggplot(data = world) +
        geom_sf() +
        coord_sf(xlim = c(110, 155), ylim = c(-40, -10)) +
        theme(panel.background = element_rect(fill = "aliceblue")) + #coloring the ocean blue
        geom_jitter(data = my_species, height=.03,width=.035,aes(x = lon, y = lat), 
                    size = 3, shape = 21, alpha = 1, fill = factor(my_species$color_group))
        
map1
```

![](Map_examples_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
##specifying colors in the code using hex codes (and adding a title) - by genus
map2 <- ggplot(data = world) +
        geom_sf() +
        coord_sf(xlim = c(110, 155), ylim = c(-40, -10)) +
        theme(panel.background = element_rect(fill = "aliceblue")) +
        geom_jitter(data = my_species, height=.03,width=.035,aes(x = lon, y = lat, fill=genus), 
                        size = 2, shape = 21, alpha = 1.0) + scale_fill_manual(values = c("#004B00", "#009BFF", "#00AAA7", "#00DFE9", "#FF55F7")) +
                        labs(title = "Australian Pettalidae by genus") 

map2
```

![](Map_examples_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

``` r
#automatically color by a standard gradient (without having to specify individually) - by species
map3 <- ggplot(data = world) +
        geom_sf() +
        coord_sf(xlim = c(110, 155), ylim = c(-40, -10)) +
        theme(panel.background = element_rect(fill = "aliceblue")) +
        geom_jitter(data = my_species, height=.03,width=.035,aes(x = lon, y = lat, fill=species), 
                        size = 2, shape = 21, alpha = 1.0) + scale_fill_hue() +
                        labs(title = "Australian Pettalidae by species") 

map3
```

![](Map_examples_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
##command below can be used to save your map as a pdf
#ggsave("map2.pdf", width = 17, height = 12, units = "cm", dpi = 300)
```
