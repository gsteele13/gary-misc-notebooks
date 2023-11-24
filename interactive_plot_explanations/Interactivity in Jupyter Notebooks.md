---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.15.2
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Interactivity in Jupyter Notebooks


## Interacting with a function: Matplotlib inline + ipywidgets

Works, but is sloooowwwww...

```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact

x = np.linspace(-10,10,100)
def f(x, a, b):
    return a*x**2 + b

def do_plot(a=1,b=0):
    plt.plot(x,f(x,a,b))
    plt.grid()

interact(do_plot,a=(0,10,0.1),b=(-20,20,0.1))
```

## Bokeh: interactive zooming / panning

More lines of import code, more "boilerplate" (4 lines instead of just plt.plot()) but interactive already.

```python
import numpy as np

from bokeh.models import ColumnDataSource
from bokeh.plotting import figure, show
from bokeh.io import output_notebook
output_notebook()

def f(x, a, b):
    return a*x**2 + b
x = np.linspace(-10,10,100)
a=1; b=0

# Plot it
p = figure(height=400, width=600) 
p.line(x,f(x,a,b))
show(p)
```

## Bokeh interacting with a function

```python
import numpy as np
from bokeh.models import ColumnDataSource
from bokeh.plotting import figure, show
from bokeh.io import output_notebook, push_notebook
output_notebook()
from ipywidgets import interact


def f(x, a, b):
    return a*x**2 + b
x = np.linspace(-10,10,100)
a=1; b=0

# Plot it
p = figure(height=400, width=600) 
l = p.line(x,f(x,a,b))
target = show(p, notebook_handle=True)

def update_plot(a=a, b=b):
    l.data_source.data = dict(x=x, y=f(x,a,b))
    push_notebook(handle=target)
    
interact(update_plot, a=(0,10,0.1),b=(-20,20,0.1));
```

```python

```
