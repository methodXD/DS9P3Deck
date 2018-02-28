---
title       : DS9P3 - Shiny Application
subtitle    : Mtcars Linear Model and Variable Correlation
author      : Jeremy Stalberger
job         : February 28, 2018
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Shiny Application

### Purpose

This application allows the user to investigate the relationship between variables in the Motor Trend Car Road Tests (mtcars) dataset through the use of variable drop-down boxes and the resulting scatter plot and linear models.

### Data

The mtcars dataset was extracted from the 1974 Motor Trends US magazine and comprises fuel consumption and 10 aspects of automobile design and performance for 32 automobiles (1973-74 models).

---

## UI.R Overview

### Sidebar
In the sidebar, there are two drop-down boxes to select the Y and X variables to be plotted in the scatterplot. By default, the 'mpg' is selected as the Y variable and 'hp' is selected as the X variable. There is also a checkbox to select whether to show or hide the resulting linear model in the plot.

### Main Panel
The main panel displays a scatterplot between the two variables selected in the sidebar. A reactive function will calculate a linear model and display this line in red on the plot. There are three calculations reactively provided below the plot: 
- The correlation between the variables
- The slope of the linear model
- The intercept of the linear model

---

## Server.R Code


```r
shinyServer(function(input, output){
    model <- reactive({
        lm(mtcars[,input$Yvar] ~ mtcars[,input$Xvar])})
    output$plot1 <- renderPlot({
        plot(mtcars[,input$Xvar], mtcars[,input$Yvar], xlab = input$Xvar,
             ylab = input$Yvar, bty = "n", pch = 16,
             xlim = c(floor(min(mtcars[,input$Xvar])), ceiling(max(mtcars[,input$Xvar]))),
             ylim = c(floor(min(mtcars[,input$Yvar])), ceiling(max(mtcars[,input$Yvar]))))
        if(input$showModel){
            abline(model(), col = "red", lwd = 2)}})
    output$corr1 <- renderText({
        cor(mtcars[,input$Yvar],mtcars[,input$Xvar])})
    output$coef2 <- renderText({
        model()$coef[2]})
    output$coef1 <- renderText({
        model()$coef[1]})
})
```

--- &twocol

## Plot Preview

*** =left


```r
model <- lm(mtcars[,"mpg"]~mtcars[,"hp"])
                                         
plot(mtcars[,"hp"],                      
     mtcars[,"mpg"],                     
     xlab = "hp",                        
     ylab = "mpg",                       
     bty = "n",                          
     pch = 16,                           
     xlim=c(floor(min(mtcars[,"hp"])),   
          ceiling(max(mtcars[,"hp"]))),  
     ylim=c(floor(min(mtcars[,"mpg"])),  
          ceiling(max(mtcars[,"mpg"])))) 
                                         
abline(model, col = "red", lwd = 2)      
```

*** =right

![plot of chunk unnamed-chunk-3](assets/fig/unnamed-chunk-3-1.png)


