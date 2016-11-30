---
---

## Solution 1


~~~r
# Libraries
library(ggplot2)
library(dplyr)

# Data
species <- read.csv("data/species.csv", stringsAsFactors = FALSE)
surveys <- read.csv("data/surveys.csv", na.strings = "", stringsAsFactors = FALSE)

# User Interface
in1 <- selectInput("pick_species",
                   label = "Pick a species",
                   choices = unique(species["species_id"]))
out1 <- textOutput("species_name")
out2 <- plotOutput("species_plot")
tab <- tabPanel("Species", in1, out1, out2)
ui <- navbarPage(title = "Portal Project", tab)

server <- function(input, output) {
  output[["species_name"]] <- renderText(
    species %>%
      filter(species_id == input[["pick_species"]]) %>%
      select(genus, species) %>%
      paste(collapse = ' ')
  )
  output[["species_plot"]] <- renderPlot(
    surveys %>%
      filter(species_id == input[["pick_species"]]) %>%
      ggplot(aes(year)) +
      geom_bar()
  )
}

# Create the Shiny App
shinyApp(ui = ui, server = server)
~~~
{:.text-document title="lesson-6-3.R"}

<aside class="notes" markdown="block">

[Return](#exercise-1)

</aside>

<!--split-->


## Solution 2


~~~r
# Libraries
library(ggplot2)
library(dplyr)

# Data
species <- read.csv("data/species.csv", stringsAsFactors = FALSE)
surveys <- read.csv("data/surveys.csv", na.strings = "", stringsAsFactors = FALSE)

# User Interface
in1 <- selectInput("pick_species",
                   label = "Pick a species",
                   choices = unique(species["species_id"]))
side <- sidebarPanel("Options", in1)
out1 <- textOutput("species_name")
tab1 <- tabPanel("Plot",
                 plotOutput("species_plot"))
tab2 <- tabPanel("Data",
                 dataTableOutput("species_table"))                 
out2 <- tabsetPanel(tab1, tab2)
main <- mainPanel(out1, out2)
tab <- tabPanel("Species",
                sidebarLayout(side, main))
ui <- navbarPage(title = "Portal Project", tab)

server <- function(input, output) {
  output[["species_name"]] <- renderText(
    species %>%
      filter(species_id == input[["pick_species"]]) %>%
      select(genus, species) %>%
      paste(collapse = ' ')
  )
  output[["species_plot"]] <- renderPlot(
    surveys %>%
      filter(species_id == input[["pick_species"]]) %>%
      ggplot(aes(year)) +
      geom_bar()
  )
  output[["species_table"]] <- renderDataTable(
    surveys %>%
      filter(species_id == input[["pick_species"]])
  )
}

# Create the Shiny App
shinyApp(ui = ui, server = server)
~~~
{:.text-document title="lesson-6-3.R"}

<aside class="notes" markdown="block">

[Return](#exercise-2)

</aside>

<!--split-->

## Solution 3


~~~r
# Libraries
library(ggplot2)
library(dplyr)

# Data
species <- read.csv("data/species.csv", stringsAsFactors = FALSE)
surveys <- read.csv("data/surveys.csv", na.strings = "", stringsAsFactors = FALSE)

# User Interface
in1 <- selectInput("pick_species",
                   label = "Pick a species",
                   choices = unique(species["species_id"]))
side <- sidebarPanel(h2("Options", align="center"), in1)
out1 <- textOutput("species_name")
tab1 <- tabPanel("Plot",
                 plotOutput("species_plot"))
tab2 <- tabPanel("Data",
                 dataTableOutput("species_table"))                 
out2 <- tabsetPanel(tab1, tab2)
main <- mainPanel(out1, out2)
tab <- tabPanel("Species",
                sidebarLayout(side, main))
ui <- navbarPage(title = "Portal Project", tab)

server <- function(input, output) {
  output[["species_name"]] <- renderText(
    species %>%
      filter(species_id == input[["pick_species"]]) %>%
      select(genus, species) %>%
      paste(collapse = ' ')
  )
  output[["species_plot"]] <- renderPlot(
    surveys %>%
      filter(species_id == input[["pick_species"]]) %>%
      ggplot(aes(year)) +
      geom_bar()
  )
  output[["species_table"]] <- renderDataTable(
    surveys %>%
      filter(species_id == input[["pick_species"]])
  )
}

# Create the Shiny App
shinyApp(ui = ui, server = server)
~~~
{:.text-document title="lesson-6-3.R"}

<aside class="notes" markdown="block">

[Return](#exercise-3)

</aside>

<!--split-->

## Solution 4


~~~r
# Libraries
library(ggplot2)
library(dplyr)

# Data
species <- read.csv("data/species.csv", stringsAsFactors = FALSE)
surveys <- read.csv("data/surveys.csv", na.strings = "", stringsAsFactors = FALSE)

# User Interface
in1 <- selectInput("pick_species",
                   label = "Pick a Species",
                   choices = unique(species[["species_id"]]))
in2 <- sliderInput("slider_months",
                   label = "Month Range",
                   min = 1,
                   max = 12,
                   value = c(1, 12))
dl <- downloadButton("download_data", label = "Download")
side <- sidebarPanel(h3("Options", align="center"), in1, in2, dl)
out2 <- plotOutput("species_plot")
main <- mainPanel(out2)
tab <- tabPanel("Species",
sidebarLayout(side, main))
ui <- navbarPage("Portal Project", tab)

# Server
server <- function(input, output) {
  reactive_range <- reactive(
    seq(input[["slider_months"]][1],
        input[["slider_months"]][2])
  )
  reactive_data <- reactive(
    surveys %>%
      filter(species_id == input[["pick_species"]]) %>%
      filter(month %in% reactive_range())
  )
  output[["species_plot"]] <- renderPlot(
   reactive_data() %>%
      ggplot(aes(year)) +
      geom_bar()
  )
output[["download_data"]] <- downloadHandler(
  filename = "species.csv",
  content = function(file) {
    reactive_data() %>%
      write.csv(file)
  })
}

# Create the Shiny App
shinyApp(ui = ui, server = server)
~~~
{:.text-document title="lesson-6-4.R"}

<aside class="notes" markdown="block">

[Return](#exercise-4)

</aside>