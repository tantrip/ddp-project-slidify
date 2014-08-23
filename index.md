---
title       : Developing Data Products Project Presentation
subtitle    : Map of major crimes across the United States by selected year
author      : tantrip
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Project Description

### Map of major crimes across the United States by selected year

#### This Shiny and rCharts demo shows crime statistics for various US states from 1969 to 2010  
  * The map will change dynamically based on your selections as detailed below   
    - Use the slider to select an year   
    - Select a value from dropdown to change color palette for the map   
    - Select number of breaks to increase or decrease the definition of color palette   
    - Shiny server code will slice the crime dataset for selected year and show the chloropleth map   
    - Hover on any state on the map to see the number of crimes in that state for the currently selected year   

##### Please note: This demo has drawn from some of the examples from Ramnath (author of rCharts) with updates and additions

--- .class #id




