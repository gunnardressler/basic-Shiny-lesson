---
---

## Reactivity

Input objects are **reactive** which means that an update to this value by a user will notify objects in the server that its value has been changed.

The outputs of render functions are called **observers** because they observe all "upstream" reactive values for changes.

<!--split-->

## Reactive objects

The code inside the body of `render*()` functions will re-run whenever a reactive value inside the code changes, such as when an input object's value is changed in the UI.
The input object notifies its observers that it has changed, which causes the output objects to re-render and update the display. 

Question
: Which element is an **observer** in the app within `lesson-6-3.R`.

Answer
: The object created by `renderPlot()` and stored with outputId "species_plot".

<!--split-->

In `lesson-6-4.R` we're going to create a new input object in the sidebar panel that constrains the plotted data to a user defined range of months.


~~~r
in2 <- sliderInput("slider_months",
                   label = "Month Range",
                   min = 1,
                   max = 12,
                   value = c(1, 12))
side <- sidebarPanel(h3("Options", align="center"), in1, in2)									    
~~~
{:.text-document title="lesson-6-4.R"}

<!--split-->

To limit surveys to the user specified months, an additional filter is needed that is something like


~~~r
filter(month %in% %reactive_range%)
~~~
{:.input}

<!--split-->

The way we will define the reactive_range within the server makes it a function, so we must call it when used in the observer definition.


~~~r
# Server
server <- function(input, output) {

  reactive_range <- ...

  output[["species_plot"]] <- renderPlot(
    surveys %>%
    filter(species_id == input[["pick_species"]]) %>%
    filter(month %in% reactive_range()) %>%
    ggplot(aes(year)) +
    geom_bar()
  )
}
~~~
{:.text-document title="lesson-6-4.R"}

<!--split-->

The function `reactive()` creates generic objects for use by observers.


~~~r
# Server
server <- function(input, output) {
   reactive_range <- reactive(
    seq(input[["slider_months"]][1],
        input[["slider_months"]][2])
    )
    
  output[["species_plot"]] <- renderPlot(
    surveys %>%
    filter(species_id == input[["pick_species"]]) %>%
    filter(month %in% reactive_range()) %>%
    ggplot(aes(year)) +
    geom_bar()
  )
}
~~~
{:.text-document title="lesson-6-4.R"}