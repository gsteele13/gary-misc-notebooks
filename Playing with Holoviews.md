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

# Playing around with Holoviews

While I try to figure out how to get the Mandelbrot example running in a notebook, I'll play around with some more examples. 

Let's start with this one:

https://holoviews.org/user_guide/Deploying_Bokeh_Apps.html

```python
import numpy as np
import holoviews as hv
hv.extension('bokeh')

# Declare some points
points = hv.Points(np.random.randn(1000,2 ))

# Declare points as source of selection stream
selection = hv.streams.Selection1D(source=points)

# Write function that uses the selection indices to slice points and compute stats
def selected_info(index):
    arr = points.array()[index]
    if index:
        label = 'Mean x, y: %.3f, %.3f' % tuple(arr.mean(axis=0))
    else:
        label = 'No selection'
    return points.clone(arr, label=label).opts(color='red')

# Combine points and DynamicMap
selected_points = hv.DynamicMap(selected_info, streams=[selection])
layout = points.opts(tools=['box_select', 'lasso_select']) + selected_points

layout
```

OK, this works "out of box". Let's try some more. 

```python

```
