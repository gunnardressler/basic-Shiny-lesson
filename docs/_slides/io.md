---
---

## Input and Output Objects

The user interface and the server interact with each other through **input** and **output** objects.
The user's interaction with input objects alters parameters in the server's instructions -- instructions for creating output objects shown in the UI.

Writing an app requires careful attention to how your input and output objects relate to each other, i.e. knowing what actions will initiate what sections of code to run at what time.

<!--split-->

![]({{ site.baseurl }}/images/arrows3.png)

This diagrams input and output relationships within the UI and server objects:

- Input objects are created and named in the UI with functions like `selectInput()` or `radioButtons()`. 
- Data from input objects affects render functions in the server object which create output objects. 
- Output objects are placed in the UI using output functions like `plotOutput()` or `textOutput()`.

<!--split-->

## Input Objects

Input objects collect information from the user and save it into a list.
Input values can change when a user changes the input.
These inputs can be many different things: single values, text, vectors, dates, or even files uploaded by the user. 

The first two arguments for all input objects are:

- `inputId` (notice the capitalization here!) which is for giving the input object a name to refer to in the server, and 
- `label` which is for text to display to the user. 

<aside class="notes" markdown="block">

See a gallery of input objects with sample code [here](http://shiny.rstudio.com/gallery/widget-gallery.html)

</aside>

<!--split-->

Define the UI by adding an input object that lets users select a species ID from the species table.


~~~r
# User Interface
in1 <- selectInput(inputID = "pick_species",
                   label = "Pick a species",
                   choices = unique(species[["species_id"]]))
...
tab <- tabPanel("Species", in1, ...)
ui <- navbarPage(title = "Portal Project", tab)
~~~
{:.text-document title="lesson-6-2.R"}

<aside class="notes" markdown="block">

Use the `selectInput()` function to create an input object called `pick_species`.
Use the `choices = ` argument to define a vector with the unique values in the species id column.
Make the input and output objects arguments to the function `tabPanel()`, preceded by a title argument.
We will learn about design and layout in a subsequent section.

**Other notes about input objects**

- Choices for inputs can be named using a list to match the display name to the value such as `list("Male" = "M", "Female" = "F")`. 
- [Selectize](http://shiny.rstudio.com/gallery/selectize-vs-select.html) inputs are a useful option for long drop down lists.
- Always be aware of what the default value is for input objects you create.

</aside>

<!--split-->

## Output objects

Output objects are created through the combination of pairs of `render*()` and `*Output()` functions, in the UI and server respectively.

| Desired UI Object | `render*()`       | `*Output()`           |
|-------------------+-------------------+----------------------|
| plot              | renderPlot()      | plotOutput()         |
| text              | renderPrint()     | verbatimTextOutput() |
| text              | renderText()      | textOutput()         |
| static table      | renderTable()     | tableOutput()        |
| interactive table | renderDataTable() | dataTableOutput()    |

The server function adds the output of the `render*()` functions to a list of output objects.

<!--split-->

## Textual Output

Display the species id as text under the input object using `textOutput` in the UI and `renderText` in the server object.


~~~r
# User Interface
in1 <- selectInput(inputId = "pick_species",
                   label = "Pick a species",
                   choices = unique(species[["species_id"]]))
out1 <- textOutput("species_id")
tab <- tabPanel("Species", in1, out1)
ui <- navbarPage(title = "Portal Project", tab)

# Server
server <- function(input, output) {
  output[["species_id"]] <- renderText(input[["pick_species"]])
}
~~~
{:.text-document title="lesson-6-2.R"}

Go ahead and **run the app!**

<aside class="notes" markdown="block">

Render functions tell Shiny how to build an output object to display in the user interface.
Output objects can be data frames, plots, images, text, or most anything you can create with R code to be visualized. 

Use `outputId` names in quotes to refer to output objects within `*Output()` functions. Other arguments to `*Output()` functions can control their size in the UI as well as add advanced interactivity such as [selecting observations to view data by clicking on a plot](http://shiny.rstudio.com/articles/selecting-rows-of-data.html).

Note that it is also possible to render reactive input objects using the `renderUI()` and `uiOutput()` functions for situations where you want the type or parameters of an input object to change based on another input. For an exmaple, see "Creating controls on the fly" [here](http://shiny.rstudio.com/articles/dynamic-ui.html).

</aside>

<!--split-->

## Graphical Output

In `lesson-6-3.R`, we use the surveys table to plot abundance of the selected species, rather than just printing its id.

First, the server must filter the survey data based on the selected species, and then create a bar plot **within** the `renderPlot()` function.
Don't forget to import the necessary libraries.


~~~r
server <- function(input, output) {
  output[["species_plot"]] <- renderPlot(
    surveys %>%
    filter(species_id == input[["pick_species"]]) %>%
    ggplot(aes(year)) +
    geom_bar()
  )
}
~~~
{:.text-document title="lesson-6-3.R"}

<!--split-->

Second, use the corresponding `plotOutput()` function in the UI to display the plot in the app. 


~~~r
# User Interface
in1 <- selectInput(inputId = "pick_species",
                   label = "Pick a species",
                   choices = unique(species["species_id"]))
out2 <- plotOutput("species_plot")
tab <- tabPanel("Species", in1, out2)
ui <- navbarPage(title = "Portal Project", tab)
~~~
{:.text-document title="lesson-6-3.R"}

<!--split-->

## Exercise 1

Use `textOutput()` to add a title above the plot giving the full species name. The function `paste()` with argument `collapse = ' '` will convert a data frame row to a text string. Hint: Multiple `render*()` functions are allowed in the server function.
