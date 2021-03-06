---
---

## Lesson Goals

This lesson presents an introduction to creating interactive web applications
using the R [Shiny](https://cran.r-project.org/web/packages/shiny/index.html)
package. It covers:

- basic building blocks of a Shiny app
- how to display interactive elements & plots
- how to customize and arrange elements on a page

===

## Data

This lesson makes use of several publicly available datasets that have been
customized for teaching purposes, including the [Population estimates from the U.S. Census Bureau](https://www.census.gov/programs-surveys/popest.html).

===

## What is Shiny?

Shiny is a web application framework for R that allows you to create interactive
web apps without requiring knowledge of HTML, CSS, or JavaScript. These web apps
can be used for exploratory data analysis and visualization, to facilitate
remote collaboration, share results, and [much
more](http://shiny.rstudio.com/gallery/). The example below, and many
[additional examples](https://shiny.rstudio.com/tutorial/written-tutorial/lesson1/#Go Further),
will open in a new browser window (you may need to prevent your broswer from
blocking pop-out windows in order to view the app).
{:.notes}

The [shiny](){:.rlib} package includes some built-in examples to demonstrate
some of its basic features. When applications are running, they are displayed in
a separate browser window or the RStudio Viewer pane.




~~~r
> library(shiny)
> runExample('01_hello')
~~~
{:title="Console" .no-eval .input}

```

Listening on http://127.0.0.1:4321
```
{:.output .message}

===

Notice back in RStudio that a stop sign appears in the Console window while your
app is running. This is because the current R session is busy running your
application.

Closing the app window may not stop the app from using your R session. Force the
app to close when necessary by clicking the stop sign in the header of the
Console window. The Console window prompt `>` should return.
{:.notes}
