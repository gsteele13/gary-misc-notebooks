---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.1'
      jupytext_version: 1.2.2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

## Interact
Here is a simple example of using the `interact` decorator from ipywidgets to create a simple set of widgets to control the parameters of a plot

```python
import plotly.graph_objs as go
import numpy as np
from ipywidgets import interact
```

First we'll create an empty figure, and add an empty scatter trace to it.

```python
fig = go.FigureWidget()
scatt = fig.add_scatter()
scatt = fig.data[0]

```

```python
xs=np.linspace(0, 6, 100)

@interact(a=(1.0, 4.0, 0.01), b=(0, 10.0, 0.01), color=['red', 'green', 'blue'])
def update(a=3.6, b=4.3, color='blue'):
    with fig.batch_update():
        scatt.x=xs
        scatt.y=np.sin(a*xs-b)
        scatt.line.color=color

fig
```

```python

```
