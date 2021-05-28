---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.10.1
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import numpy as np
import matplotlib.pyplot as plt
import time

from bokeh.plotting import figure, show
from bokeh.io import output_notebook, push_notebook
from bokeh.models import ColumnDataSource, Toggle, Range1d
from bokeh.layouts import column, row

import ipywidgets as widgets

import IPython
ipython = IPython.get_ipython()

output_notebook()
```

# Starting point: live plot

Add a stop button. Use a while loop for the event loop. 

```python
x = np.linspace(-10,10,100)
def f(x):
    return np.sin(x)+np.random.normal(size=len(x))*0.1

source = ColumnDataSource()
source.data = dict(x=x, y=f(x))

p = figure(title="test", plot_height=300, plot_width=500)
l = p.line('x', 'y', source=source)
target = show(p, notebook_handle=True)

stop_button = widgets.ToggleButton(description='Stop')
display(stop_button)

while True:
    ipython.kernel.do_one_iteration()
    if stop_button.value:
        break 
    l.data_source.data['y'] = f(x)
    push_notebook(handle=target)
    time.sleep(0.01)
```

# Plot to fill output width

Seems to be controlled by a parameter `sizing_mode`, which can be specified by a single plot or by a row/column/grid container? 

https://docs.bokeh.org/en/latest/docs/reference/layouts.html


```python
x = np.linspace(-10,10,100)
def f(x):
    return np.sin(x)+np.random.normal(size=len(x))*0.1

source = ColumnDataSource()
source.data = dict(x=x, y=f(x))

p = figure(title="test", height=300, width=600)
l = p.line('x', 'y', source=source)
target = show(p, notebook_handle=True)

# This one make sense, it is not so 100% clear what the height should be
# in an output cell...?
p.sizing_mode = "stretch_width"

# This is also a good option
p.sizing_mode = "scale_width"

stop_button = widgets.ToggleButton(description='Stop')
display(stop_button)

while True:
    ipython.kernel.do_one_iteration()
    if stop_button.value:
        break 
    l.data_source.data['y'] = f(x)
    push_notebook(handle=target)
    time.sleep(0.01)
```

# Now with columns

```python
x = np.linspace(-10,10,100)
def f(x):
    return np.sin(x)+np.random.normal(size=len(x))*0.1

source = ColumnDataSource()
source.data = dict(x=x, y=f(x))

p = figure(title="test", height=300, width=600)
l = p.line('x', 'y', source=source)
p2 = figure(title="test", height=300, width=600)
l2 = p2.line('x', 'y', source=source)

r = row(p,p2)
for w in p, p2, r:
    w.sizing_mode = "scale_width"
target = show(r, notebook_handle=True)

stop_button = widgets.ToggleButton(description='Stop')
display(stop_button)

while True:
    ipython.kernel.do_one_iteration()
    if stop_button.value:
        break 
    l.data_source.data['y'] = f(x)
    push_notebook(handle=target)
    time.sleep(0.01)
```

# Now two columns and two rows

```python
x = np.linspace(-10,10,100)
def f(x):
    return np.sin(x)+np.random.normal(size=len(x))*0.1

source = ColumnDataSource()
source.data = dict(x=x, y=f(x))

p = figure(title="test", height=300)
l = p.line('x', 'y', source=source)
p2 = figure(title="test", height=300)
l2 = p2.line('x', 'y', source=source)
p3 = figure(title="test", height=300)
l3 = p3.line('x', 'y', source=source)

r = row(p,p2)
c = column(r,p3)
for w in p, p2, p3, r, c:
    w.sizing_mode = "stretch_width"
target = show(c, notebook_handle=True)

stop_button = widgets.ToggleButton(description='Stop')
display(stop_button)

while True:
    ipython.kernel.do_one_iteration()
    if stop_button.value:
        break 
    l.data_source.data['y'] = f(x)
    push_notebook(handle=target)
    time.sleep(0.01)
```

```python

```
