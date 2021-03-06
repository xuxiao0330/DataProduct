Group Insurance Project 
========================================================
author: Xiao  
date: February 7th, 2016

First Slide
========================================================

We need two elements to calculate a group insurance policy.

- Average Expenses Associtaed (per thousand USD insurable amount)
- Total Insurable Amount

New policy tend to have a bit higher risk in the first year. 

A 10% Risk Margin for new policy is also introduced.

Input Code
========================================================
A glance at Input Code.

```r
library(shiny)

shinyUI(fluidPage(

  # Application title
  titlePanel("Group Insurance Pricing Project"),

  # Sidebar Input for Average Expenses/1000USD, Insurable Amount and New Policy.
  # We introduce a 10% additional risk margin for new policy.
  sidebarLayout(
    sidebarPanel(
      sliderInput("avgexp",
                  "Average expenses per thousand USD in 2016:",
                  min = 50,
                  max = 500,
                  value = 50,
                  step = 10),
      sliderInput("ta",
                  "Total Insurable Amount in Policy:",
                  min = 1000,
                  max = 200000,
                  value = 5000,
                  step = 1000,
                  pre = "$"),
      checkboxInput("np",
                    "Is this a new policy?",
                    value = FALSE)
      
      ),
  
    # Show a plot of the generated distribution
    mainPanel(
      textOutput("text1"),
      textOutput("text2"),
      textOutput("text3")
    )
  )
))
```

Output Code
========================================================


```r
shinyServer(function(input, output) {

  output$text1 <- renderText({
    
    paste("Your estimated expense rate is:",input$avgexp/1000)
  })
  output$text2 <- renderText({
    
    paste("Your total amount insured is",input$ta)
  })
  output$text3 <- renderText({
    paste("Your final premium is",ifelse(input$np=="TRUE",
  input$avgexp/1000*input$ta*1.1,input$avgexp/1000*input$ta))
  })
})
```

Final Premium Output
========================================================
Final Premium is calculated by the following formula:

Average Expense Rate * Total Insurable Amount * (1 + Risk Margin)

This is an easy group pricing app. 

Add'l features like geographic, demographic factors can be added further.

