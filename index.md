---
title       : Developing Data Products Project Presentation
subtitle    : Map of major crimes across the United States by selected year
author      : tantrip
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [rCharts, rMaps, PerformanceAnalytics, reshape2] # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Project Description

### Map of major crimes across the United States by selected year

  * This Shiny and rCharts demo shows crime statistics for various US states from 1969 to 2010  
  * The map will change dynamically based on your selections as detailed below   
    - Use the slider to select an year   
    - Select a value from dropdown to change color palette for the map   
    - Select number of breaks to increase or decrease the definition of color palette   
    - Shiny server code will slice the crime dataset for selected year and shows the map   
    - Hover on any state on the map to see the number of crimes in that state for the currently selected year   

  * Please note: This demo has drawn from some of the examples from Ramnath (author of rCharts) with updates and additions

##### The link to slideapps.io to see the project in action is: [Project Link](http://tantrip.shinyapps.io/ddp-project/)

--- .class

#### Slidify is a great tool! Here is how it works..

The following code block gets executed and will show the plot (using ggplot2) right below the code!


```r
library(ggplot2)
output <- rnorm(200, 100, 20)
qplot(output)
```

```
## stat_bin: binwidth defaulted to range/30. Use 'binwidth = x' to adjust this.
```

![plot of chunk block2](assets/fig/block2.png) 

--- .class #id


#### ui.R code for the assignment

As you can see below, the UI provides a set of controls on the left that allow you to select the year, color scheme and the number of splits in the color scheme.



```r
shinyUI(fluidPage(
  fluidRow(column(12, '')),
  fluidRow(column(12, wellPanel(tags$h4("Map of major crimes across the United States by selected year")))),
  fluidRow(column(3, wellPanel(sliderInput("year", tags$b("Select Year:"), min = 1969, max = 2010, value = 1995, 
                                           step = 1),
      tags$br(), tags$br(),
      selectInput("pal", tags$b('Select Color Palette'), c('Blues', 'Accent', 'PuRd', 'Purples', 'OrRd', 'Oranges', 'Greys')),
      tags$br(), tags$br(),
      sliderInput("ncuts", tags$b("Select Number of Breaks:"), min = 3, max = 8, value = 7, step = 1),
      tags$br(), tags$br())),
    column(8, rCharts::chartOutput('myplot', 'datamaps'))),
fluidRow(
  column(12, wellPanel(tags$h4("This Shiny and rCharts demo shows crime statistics for various US states from 1969 to 2010"),
    tags$h5("The map will change dynamically based on your selections as detailed below"),
    tags$ul(tags$li("Use the slider to select an year"),
      tags$li("Select a value from dropdown to change color palette for the map"),
      tags$li("Select number of breaks to increase or decrease the definition of color palette"),
      tags$li("Shiny server code will slice the crime dataset for selected year and show the chloropleth map"),
      tags$li("Hover on any state on the map to see the number of crimes in that state for the currently selected year")
    ),
    tags$h5("Please note: This demo has drawn from some of the examples from Ramnath (author of rCharts) with updates and additions"))))))
```

--- class #id

#### server.R code for the assignment

As you can see this code takes the input values from ui.R and plots a choropleth map with the crime statistics for the selected year, across all US states. The popover shows the actual value for each state along with the state name.



```r
require(rCharts)
require(rMaps)

shinyServer(function(input, output, session){
  
  output$myplot = rCharts::renderChart2({ichoropleth(
    Crime ~ State, 
    data = subset(violent_crime, Year == input$year),
    pal = input$pal,
    ncuts = input$ncuts,
    geographyConfig=list(popupTemplate="#!function(geo, data) {
                                                              return '<div class=\"hoverinfo\"><strong>'+
                                                                        geo.properties.name +
                                                                        '<br>' + 
                                                                        'Total violent crimes: ' + 
                                                                        data.Crime +
                                                                        '</strong></div>';
                                                              }!#")
  )})
}
)
```
