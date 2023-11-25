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

interact(do_plot,a=(0,10,0.1),b=(-20,20,0.1));
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

## Another nice example: Matplotlib JSHTML animations

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
plt.rcParams["animation.html"] = "jshtml"

x = np.linspace(0,2*np.pi,100)
t = np.linspace(0,2*np.pi,100)

def f(x,t):
    return np.sin(x)*np.cos(t)

fig, ax = plt.subplots()
line, = ax.plot(x,f(x,t[0]));
plt.axhline(0,ls=":",c='grey')
title = plt.title("t = %s seconds" % t[0])

def update(i):
    title.set_text("t = %.2f seconds" % t[i])
    line.set_data(x,f(x,t[i]))

anim = animation.FuncAnimation(fig, update, frames=len(t), interval = 40)
plt.close(fig)
```

```python
anim
```

# What happens when I export to HTML? 

```python
!jupyter nbconvert --to html --embed-images "Interactivity in Jupyter Notebooks.ipynb"
```

# One solution: Javascript functions, Bokeh sliders

From:

https://docs.bokeh.org/en/2.4.1/docs/gallery/slider.html

```python
import numpy as np

from bokeh.layouts import column, row
from bokeh.models import CustomJS, Slider
from bokeh.plotting import ColumnDataSource, figure, show

x = np.linspace(0, 10, 500)
y = np.sin(x)

source = ColumnDataSource(data=dict(x=x, y=y))

plot = figure(y_range=(-10, 10), width=400, height=400)

plot.line('x', 'y', source=source, line_width=3, line_alpha=0.6)

amp_slider = Slider(start=0.1, end=10, value=1, step=.1, title="Amplitude")
freq_slider = Slider(start=0.1, end=10, value=1, step=.1, title="Frequency")
phase_slider = Slider(start=0, end=6.4, value=0, step=.1, title="Phase")
offset_slider = Slider(start=-5, end=5, value=0, step=.1, title="Offset")

callback = CustomJS(args=dict(source=source, amp=amp_slider, freq=freq_slider, phase=phase_slider, offset=offset_slider),
                    code="""
    const data = source.data;
    const A = amp.value;
    const k = freq.value;
    const phi = phase.value;
    const B = offset.value;
    const x = data['x']
    const y = data['y']
    for (let i = 0; i < x.length; i++) {
        y[i] = B + A*Math.sin(k*x[i]+phi);
    }
    source.change.emit();
""")

amp_slider.js_on_change('value', callback)
freq_slider.js_on_change('value', callback)
phase_slider.js_on_change('value', callback)
offset_slider.js_on_change('value', callback)

layout = row(
    plot,
    column(amp_slider, freq_slider, phase_slider, offset_slider),
)

show(layout)
```
