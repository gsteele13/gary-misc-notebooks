---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.1
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Bokeh Interactive Plots

In this notebook, I show some simple, minimal code for creating reactive and interactive plots in Bokeh. 

What do I mean interactive? Specifically, I consider two types of interaction: 

1. Interactively explore fixed data
    * Zoom, pan, etc, on fixed data
2. Interactively change the data that is plotted
    * For example, making a plot of a function and then changing one or more of the function parameters
    
## Purpose 1: Interactively exploring fixed data

The first I consider a fundamental and essential tool for data exploration. Although matplotlib is quite good for producing static, fixed, publication-quality plots, it is horrible (particularly in a notebook) for just "exploring" your data. While there are some attempts at this (notebook driver, and now the widget driver), they are, in my opinion, cludgy, awkward to use, and buggy. Although I hate to have to learn two different plotting APIs, in the end, the vastly superior (and much much faster) interactive data exploration experience I have in Bokeh motivated me to take this plunge. 

Does this mean that I will no longer use matplotlib? No! I still like lots of things about matplotlib. And it's great for making plots that go onto paper. I will still continue to use it for that. Bokeh is more awkward for this and there are silly things like it does not support latex. But for *exploring* my data, Bokeh wins the cake. 

## Purpose 2: Interactively exploring functions

A common task in both education, and research, is to visually explore how a function changes when I change parameters. In teaching, this is crucial for imparting visual intuition of functions onto students. For research, it is useful for example when one wants to explore how a fitting parameter changes the shape of a function you want to fit. 

For this, I have explored over many years how to do this with matplotlib. The simplest is to use ipywidgets interact function with the matplotlib inline driver. Unfortunately, this is so unbelieveably slow (several hundred millisecond delay per interaction) that it is unworkable (although a lot can be gained by throttling the interaction: the responsiveness is still poor but at least thousands of updates to not pile up when you move the mouse). A more fancy version of this can implement more responsiveness by using the widget (formerly ipympl, formerly notebook) driver. This is faster (although still an order of magnitude slower than bokeh...), and likely does some internal throttling to avoid "buildup" of updates. However, the syntax of using it is awkward, I am not a fan of the resize handles, and it also requires you to use the somewhat flaky widget driver (and the need to manually close each plot, along with the limitation of only being able to interact with one plot in your notebook!). 

Again, although I tried my best not to, I am now of the opinion that the best thing is to also use Bokeh for interactively changing plot data. I do this now in all of our lab software (with live oscilloscopes for example) and once you figure it out, it is not so much overhead code, and it really works very very well. 


# Reference code: Interactively exploring fixed data

Here is a simple reference code to generate a Bokeh plot for exploring data. I choose to scale the plot to be full width of the notebook HTML. 

```python
import numpy as np

from bokeh.models import ColumnDataSource
from bokeh.plotting import figure, show
from bokeh.io import output_notebook

# For if you have no internet connection, like on the plane...
from bokeh.resources import INLINE
output_notebook(INLINE)

# This will import the latest compatible bokeh JS from the CDN
output_notebook()
```

```python
# Generate data
x = np.linspace(0,100,1000)
y = 1/((x-50)**2 + 1) 
y += np.random.normal(size=len(x))*0.01
y += 1/((x-80)**2 + 5)

# Plot it
p = figure(height=300, width=600) 
p.sizing_mode = "scale_width"
p.line(x,y)
show(p)
```

# Reference code: Interacting with changing data

This requires a bit more work, and also some understanding of how Bokeh works. To change the data in a Bokeh plot, you need to change the data source of the line. This can be done pretty easily by keeping a copy of the line object that is created, which contains a `ColumnDataSource` object, which itself contains a data dictionary. To update the data in the line, we just have to change the data that this dictionary contains. 

That is not quite all though, as we also need to update the plot: for this, we need to have the show() command generate a notebook handle that we can later use to propagate the changes through to the plot. 

Here is a minimalistic example of how to do this:

```python
import numpy as np

from bokeh.plotting import figure, show
from bokeh.io import output_notebook, push_notebook
output_notebook()

from ipywidgets import interact
```

```python
def f(x,a,b):
    y = 1/((x-a)**2 + 1) 
    y += 1/((x-b)**2 + 5)
    return y

a = 50
b = 80

x = np.linspace(0,100,1000)
y = f(x,a,b)

p = figure(height=300, width=600) 
p.sizing_mode = "scale_width"
l = p.line(x, y)
target = show(p, notebook_handle=True)

def update_plot(a=a, b=b):
    l.data_source.data = dict(x=x, y=f(x,a,b))
    push_notebook(handle=target)
    
interact(update_plot, a=(0,200,1), b=(0,200,1));
```

```python

```
