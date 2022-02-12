---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.5
  kernelspec:
    display_name: Python [conda env:.conda-datacube_dcc]
    language: python
    name: conda-env-.conda-datacube_dcc-py
---

```python
import numpy as np

from bokeh.models import ColorBar, LogColorMapper
from bokeh.plotting import figure, output_file, show

from bokeh.io import push_notebook
from bokeh.io import output_notebook
output_notebook()
```

```python
def normal2d(X, Y, sigx=1.0, sigy=1.0, mux=0.0, muy=0.0):
    z = (X-mux)**2 / sigx**2 + (Y-muy)**2 / sigy**2
    return np.exp(-z/2) / (2 * np.pi * sigx * sigy)

X, Y = np.mgrid[-3:3:100j, -2:2:100j]
Z = normal2d(X, Y, 0.1, 0.2, 1.0, 1.0) + 0.1*normal2d(X, Y, 1.0, 1.0)
image = Z * 1e6

color_mapper = LogColorMapper(palette="Viridis256", low=1, high=1e7)

plot = figure(x_range=(0,1), y_range=(0,1), toolbar_location=None)
image = plot.image(image=[image], color_mapper=color_mapper,
           dh=[1.0], dw=[1.0], x=[0], y=[0])

color_bar = ColorBar(color_mapper=color_mapper, label_standoff=12)

plot.add_layout(color_bar, 'right')

image.glyph.color_mapper = LogColorMapper(palette="Turbo256", low=1, high=1e7)
color_bar.color_mapper = LogColorMapper(palette="Turbo256", low=1, high=1e7)

target = show(plot, notebook_handle=True)
```

```python
image.glyph.color_mapper = LogColorMapper(palette="Turbo256", low=1, high=1e7)
color_bar.color_mapper = LogColorMapper(palette="Turbo256", low=1, high=1e7)
push_notebook(handle=target)
```

```python
!bokeh info
```

```python
!jupyter --version
```

```python

```
