Title: Tutorial: Hello, Quarto – Quarto

URL Source: https://quarto.org/docs/get-started/hello/text-editor.html

Markdown Content:
### Choose your tool

*   [![Image 1](https://quarto.org/docs/get-started/images/vscode-logo.png)VS Code](https://quarto.org/docs/get-started/hello/vscode.html)
*   [![Image 2](https://quarto.org/docs/get-started/images/jupyter-logo.png)Jupyter](https://quarto.org/docs/get-started/hello/jupyter.html)
*   [![Image 3](https://quarto.org/docs/get-started/images/rstudio-logo.png)RStudio](https://quarto.org/docs/get-started/hello/rstudio.html)
*   [![Image 4](https://quarto.org/docs/get-started/images/neovim-logo.svg)Neovim](https://quarto.org/docs/get-started/hello/neovim.html)
*   [![Image 5](https://quarto.org/docs/get-started/images/text-editor-logo.png)Editor](https://quarto.org/docs/get-started/hello/text-editor.html)

Overview
--------

In this tutorial we’ll show you how to use your favorite text editor with Quarto. You’ll edit plain text `.qmd` files and preview the rendered document in a web browser as you work.

Below is an overview of how this will look.

![Image 6: On the left: A VS Code notebook titled Quarto Basics containing some text, a code cell, and the result of the code cell, which is a line plot on a polar axis. On the right: Rendered version of the same notebook.](https://quarto.org/docs/get-started/hello/images/text-editor-quarto-preview.png)

The notebook on the left is _rendered_ into the HTML version you see on the right. This is the basic model for Quarto publishing—take a source document (in this case a notebook) and render it to a variety of [output formats](https://quarto.org/docs/output-formats/all-formats.html), including HTML, PDF, MS Word, etc.

The tutorials will make use of the `matplotlib` and `plotly` Python packages—the commands you can use to install them are given in the table below.

 
| Platform | Commands |
| --- | --- |
| Mac/Linux | 
**Terminal**

```
python3 -m pip install jupyter matplotlib plotly
```





 |
| Windows | 

**Terminal**

```
py -m pip install jupyter matplotlib plotly
```





 |

Note

Note that while this tutorial uses Python, using Julia (via the [IJulia](https://julialang.github.io/IJulia.jl/stable/) kernel) is also well supported. See the article on [Using Julia](https://quarto.org/docs/computations/julia.html) for additional details.

Editor Modes
------------

If you are using VS Code, you should install the [Quarto Extension](https://marketplace.visualstudio.com/items?itemName=quarto.quarto) for VS Code before proceeding. The extension provides syntax highlighting for markdown and embedded languages, completion for embedded languages (e.g. Python, R, Julia, LaTeX, etc.), commands and key-bindings for running cells and selected line(s), and much more.

There are also Quarto syntax highlighting modes available for several other editors:

 
| Editor | Extension |
| --- | --- |
| Emacs | [https://github.com/quarto-dev/quarto-emacs](https://github.com/quarto-dev/quarto-emacs) |
| Vim / Neovim | [https://github.com/quarto-dev/quarto-vim](https://github.com/quarto-dev/quarto-vim) |
| Neovim | [https://github.com/quarto-dev/quarto-nvim](https://github.com/quarto-dev/quarto-nvim) |
| Sublime Text | [https://github.com/quarto-dev/quarto-sublime](https://github.com/quarto-dev/quarto-sublime) |

Rendering
---------

We’ll start out by rendering a simple example (`hello.qmd`) to a couple of formats. If you want to follow along step-by-step in your own environment, create a new file named `hello.qmd` and copy the following content into it.

````
---
title: "Quarto Basics"
format:
  html:
    code-fold: true
jupyter: python3
---

For a demonstration of a line plot on a polar axis, see @fig-polar.

```{python}
#| label: fig-polar
#| fig-cap: "A line plot on a polar axis"

import numpy as np
import matplotlib.pyplot as plt

r = np.arange(0, 2, 0.01)
theta = 2 * np.pi * r
fig, ax = plt.subplots(
  subplot_kw = {'projection': 'polar'} 
)
ax.plot(theta, r)
ax.set_rticks([0.5, 1, 1.5, 2])
ax.grid(True)
plt.show()
```
````

Next, open a Terminal and switch to the directory containing `hello.qmd`.

Let’s start by rendering the document to a couple of formats.

**Terminal**

```
quarto render hello.qmd --to html
quarto render hello.qmd --to docx
```

Note that the target file (in this case `hello.qmd`) should always be the very first command line argument.

When you render a `.qmd` file with Quarto, the executable code blocks are processed by Jupyter, and the resulting combination of code, markdown, and output is converted to plain markdown. Then, this markdown is processed by [Pandoc](http://pandoc.org/), which creates the finished format.

![Image 7: Workflow diagram starting with a qmd file, then Jupyter, then md, then pandoc, then PDF, MS Word, or HTML.](https://quarto.org/docs/get-started/hello/images/qmd-how-it-works.png)

Authoring
---------

The `quarto render` command is used to create the final version of your document for distribution. However, during authoring you’ll use the `quarto preview` command. Try it now from the Terminal with `hello.qmd`.

**Terminal**

```
quarto preview hello.qmd
```

This will render your document and then display it in a web browser.

![Image 8: Rendered version of the earlier notebook in a web browser.](https://quarto.org/docs/get-started/hello/images/quarto-preview.png)

You might want to position your editor and the browser preview side-by-side so you can see changes as you work.

![Image 9: Side-by-side preview of notebook on the left and live preview in the browser on the right.](https://quarto.org/docs/get-started/hello/images/text-editor-quarto-preview.png)

To see live preview in action:

1.  Change the line of code that defines `theta` as follows:
    
    ```
    theta = 4 * np.pi * r
    ```
    
2.  Save the file. The document is re-rendered, and the browser preview is updated.
    

This is the basic workflow for authoring with Quarto.

There are few different types of content in `hello.qmd`, let’s work a bit with each type.

YAML Options
------------

At the top of the file there is a YAML block with document level options.

```
---
title: "Quarto Basics"
format:
  html:
    code-fold: true
jupyter: python3
---
```

Try changing the `code-fold` option to `false`:

```
format: 
  html:
    code-fold: false
```

Then save the file. You’ll notice that the code is now shown above the plot, where previously it was hidden with a **Code** button that could be used to show it.

Markdown
--------

Narrative content is written using markdown. Here we specify a heading and a cross-reference to the figure created in the code cell below.

```
## Polar Axis

For a demonstration of a line plot on a polar axis, see @fig-polar.
```

Try changing the heading and saving the notebook—the preview will update with the new heading text.

Code Cells
----------

Code cells contain executable code to be run during render, with the output (and optionally the code) included in the rendered document.

````
```{python}
#| label: fig-polar
#| fig-cap: "A line plot on a polar axis"

import numpy as np
import matplotlib.pyplot as plt

r = np.arange(0, 2, 0.01)
theta = 2 * np.pi * r
fig, ax = plt.subplots(
  subplot_kw = {'projection': 'polar'} 
)
ax.plot(theta, r)
ax.set_rticks([0.5, 1, 1.5, 2])
ax.grid(True)
plt.show()
```
````

You are likely familiar with the Matplotlib code given here. However, there are some less familiar components at the top of the code cell: `label` and `fig-cap` options. Cell options are written in YAML using a specially prefixed comment (`#|`).

In this example, the cell options are used to make the figure cross-reference-able. Try changing the `fig-cap` and/or the code, running the cell, and then saving the file to see the updated preview.

There are a wide variety of [cell options](https://quarto.org/docs/reference/cells/cells-jupyter.html) that you can apply to tailor your output. We’ll delve into these options in the next tutorial.

Note

One particularly useful cell option for figures is `fig-alt`, which enables you to add alternative text to images for users with visual impairments. See Amy Cesal’s article on [Writing Alt Text for Data Visualization](https://medium.com/nightingale/writing-alt-text-for-data-visualization-2a218ef43f81) to learn more.

Next Up
-------

You now know the basics of creating and authoring Quarto documents. The following tutorials explore Quarto in more depth:

*   [Tutorial: Computations](https://quarto.org/docs/get-started/computations/) — Learn how to tailor the behavior and output of executable code blocks.
    
*   [Tutorial: Authoring](https://quarto.org/docs/get-started/authoring/) — Learn more about output formats and technical writing features like citations, crossrefs, and advanced layout.