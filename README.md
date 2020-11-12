
<!-- README.md is generated from README.Rmd. Please edit that file -->

<!-- used devtools::build_readme() to update the md -->

# afrilearndata

<!-- badges: start -->

<!-- badges: end -->

afrilearndata provides small African datasets to help with learning and
teaching of spatial techniques and mapping.

## Installation

Install the development version of afrilearndata with:

``` r

    # install.packages("remotes") # if not already installed
    
    remotes::install_github("afrimapr/afrilearndata")
#> Downloading GitHub repo afrimapr/afrilearndata@master
#> e1071 (1.7-3   -> 1.7-4) [CRAN]
#> Rcpp  (1.0.4.6 -> 1.0.5) [CRAN]
#> Installing 2 packages: e1071, Rcpp
#> Installing packages into 'C:/Users/andy.south/AppData/Local/Temp/RtmpmOWpuO/temp_libpath43845b725855'
#> (as 'lib' is unspecified)
#> package 'e1071' successfully unpacked and MD5 sums checked
#> package 'Rcpp' successfully unpacked and MD5 sums checked
#> 
#> The downloaded binary packages are in
#>  C:\Users\andy.south\AppData\Local\Temp\RtmpWGbNUW\downloaded_packages
#>          checking for file 'C:\Users\andy.south\AppData\Local\Temp\RtmpWGbNUW\remotes55c7f4744bd\afrimapr-afrilearndata-603bb45/DESCRIPTION' ...     checking for file 'C:\Users\andy.south\AppData\Local\Temp\RtmpWGbNUW\remotes55c7f4744bd\afrimapr-afrilearndata-603bb45/DESCRIPTION' ...   v  checking for file 'C:\Users\andy.south\AppData\Local\Temp\RtmpWGbNUW\remotes55c7f4744bd\afrimapr-afrilearndata-603bb45/DESCRIPTION' (408ms)
#>       -  preparing 'afrilearndata':
#>    checking DESCRIPTION meta-information ...     checking DESCRIPTION meta-information ...   v  checking DESCRIPTION meta-information
#>       -  checking for LF line-endings in source and make files and shell scripts
#>       -  checking for empty or unneeded directories
#>       -  building 'afrilearndata_0.0.0.9000.tar.gz'
#>      
#> 
#> Installing package into 'C:/Users/andy.south/AppData/Local/Temp/RtmpmOWpuO/temp_libpath43845b725855'
#> (as 'lib' is unspecified)
    
    library(afrilearndata)
```

## Outline

The package contains the following objects

1.  `sfafricountries` polygons, country boundaries
2.  `sfafricontinent` polygons, continent outline including madagascar
3.  `sfafrihway` lines, trans african highway network
4.  `sfafricapitals` points, capital cities
5.  `rastafriwpop` raster grid, modelled population density per square
    km aggregated to 20km squares

Lazy loading means that the objects should be accessible once
`library(afrilearndata)` is used.

If they are not recognised you can use e.g. `data(sfafricountries)` to
make sure the objects are loaded.

Firstly, here are all the data shown together. The `tmap` code to create
this plot is shown later in the readme.

    #> Warning: package 'sf' was built under R version 4.0.3
    #> Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.1

<img src="man/figures/README-tmap-all-the-data-1.png" width="100%" />

Now looking at the data layers individually plotted with packages `sf`
or `raster`

``` r

library(afrilearndata)
library(sf)

# polygons
plot(sf::st_geometry(sfafricountries))
```

<img src="man/figures/README-countries-1.png" width="100%" />

``` r

# lines
plot(sf::st_geometry(sfafrihway))
```

<img src="man/figures/README-highway-1.png" width="100%" />

``` r

# points
plot(sf::st_geometry(sfafricapitals))
```

<img src="man/figures/README-capitals-1.png" width="100%" />

``` r

# raster grid
# install.packages("raster") # if not already installed
library(raster)
#> Loading required package: sp
plot(rastafriwpop)
```

<img src="man/figures/README-population grid-1.png" width="100%" />

Interactive maps can be created using the `mapview` package.

``` r

# install.packages("mapview") # if not already installed
if (FALSE) {
  
  library(mapview)
  mapview::mapview(sfafricountries, zcol="name")  
  
}
```

Now here is a repeat of the map shown at the start of the readme,
together with the code used to create it.

``` r

library(afrilearndata)

# install.packages("tmap") # if not already installed
library(tmap)

# tmap_mode("view") to set to tmap interactive viewing mode

tm_shape(rastafriwpop) +
    tm_raster("ppp_2020_1km_Aggregated", palette = rev(viridisLite::magma(5)), breaks=c(0,2,20,200,2000,25000)) +
    #tm_raster("ppp_2020_1km_Aggregated", palette = get_brewer_pal("BuPu", n = 7), style="fisher") + 
tm_shape(sfafricountries) +
    tm_borders("white", lwd = .5) +
    #tm_text("iso_a3", size = "AREA") +
tm_shape(sfafrihway) +
    tm_lines(col = "blue") + 
tm_shape(sfafricapitals) +
    tm_symbols(col = "black", shape=1, alpha=0.4, scale = .6 ) + #shape=1 for open circle but not in view mode
tm_legend(show = FALSE)
```

<img src="man/figures/README-tmap-code-1.png" width="100%" />
