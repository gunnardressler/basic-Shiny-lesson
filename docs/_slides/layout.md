---
---

## Design and Layout

A suite of `*Layout()` functions make for a nicer user interface. You can
organize elements with a page using pre-defined high level layouts such as

- `sidebarLayout()`
- `splitLayout()`
- `verticalLayout()`

===

The more general `fluidRow()` allows any [organization of elements within a
grid](http://shiny.rstudio.com/articles/layout-guide.html#grid-layouts-in-depth).
The folowing UI elements, and more, can be layered on top of each other in
either a fluid page or pre-defined layouts.

- `tabsetPanel()`
- `navlistPanel()`
- `navbarPage()`

===

Here is a schematic of nested UI elements inside the `sidebarLayout()`. Red
boxes represent input objects and blue boxes represent output objects. Each
object is located within one or more nested **panels**, which are nested within
a **layout**. Objects and panels that are at the same level of hierarchy need to
be separated by commas in calls to parent functions. Mistakes in usage of commas
and parentheses between UI elements is one of the first things to look for when
debugging a shiny app!
{:.notes}

![]({% include asset.html path="images/layout3.png" %}){:.nobox}
{:.captioned}

===

To re-organize the elements of the "City Population" tab using a sidebar layout, we
modify the UI to specify the sidebar and main elements. Create objects `side` and
`main` then redefine the layout of `tab1`.

You may need to resize your app viewer window wider for the sidebar to appear on the left. 
{:.notes}



~~~r
side <- sidebarPanel('Options', in1)
main <- mainPanel(out1, out2)
tab1 <- tabPanel(
  title = 'City Population',
  sidebarLayout(side, main))
~~~
{:title="{{ site.data.lesson.handouts[2] }}" .no-eval .text-document}


===

## Customization

Along with input and output objects, you can add headers, text, images, links,
and other html objects to the user interface using "builder" functions. There
are shiny function equivalents for many common html tags such as `h1()` through
`h6()` for headers. You can use the console to see that the return from these
functions produce HTML code.

===



~~~r
> h5('This is a level 5 header.')
~~~
{:title="Console" .no-eval .input}

~~~
<h5>This is a level 5 header.</h5>
~~~
{:.output}




~~~r
> a(href = 'https://www.sesync.org',
+   'This renders a link')
~~~
{:title="Console" .no-eval .input}

~~~
<a href="https://www.sesync.org">This renders a link</a>
~~~
{:.output}

===

## More Layout Tips and Options

- In addition to titles for tabs, you can also use
  [icons](http://shiny.rstudio.com/reference/shiny/latest/icon.html).
- Use the argument `position = 'right'` in the `sidebarLayout()` function if you
  prefer to have the side panel appear on the right.
- See [here](http://shiny.rstudio.com/articles/tag-glossary.html) for additional
  html tags you can use.
- For large blocks of text consider saving the text in a separate markdown,
  html, or text file and use an `include*` function
  ([example](http://shiny.rstudio.com/gallery/including-html-text-and-markdown-files.html)).
- Add images by saving those files in a folder called **www**. Embed it with
  `img(src='<FILENAME>')`
- Use [shinythemes](http://rstudio.github.io/shinythemes/)!
