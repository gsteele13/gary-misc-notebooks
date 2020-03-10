---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.3.2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import numpy as np
import matplotlib.pyplot as plt
import ipywidgets as widgets
import time
```

For simulating it when we're not connected:

```python
from scipy.signal import sawtooth
N=500

def get_traces():
    time.sleep(0.1)
    t0 = np.random.random()*10
    t = np.linspace(0,15,N)
    x = sawtooth(t+t0,width=0.5)*3
    y = np.exp(x/2)
    x += np.random.normal(size=N)*0.2
    y += np.random.normal(size=N)*0.1
    return x,y,t
```

```python
x,y,t = get_traces()
plt.plot(t,x)
plt.plot(t,y)
```

## A try with matplotlib notebook update


```python
%matplotlib inline
```

```python
%matplotlib notebook
```

```python
fig,ax = plt.subplots()
x,y,t = get_traces()
line1, = ax.plot(t,x)
line2, = ax.plot(t,y)
plt.ylabel('Voltage')
plt.xlabel('Time (seconds)')
plt.show()

while True:
    x,y,t = get_traces()
    line1.set_data(t,x)
    line2.set_data(t,y)
    fig.canvas.draw()
```

The matplotlib notebook driver is a bit flaky. Why is my plot so small? And why does it not update when I drag the handle? 


## Try with Bokeh


An internet site suggested that this should work:

https://medium.com/@siZHky/update-plots-live-on-jupyter-notebook-with-bokeh-836a0b5aaf20

but it doesn't...it keeps adding more lines!

```python
from bokeh.plotting import figure, show
from bokeh.io import output_notebook, push_notebook
output_notebook()

from math import pi
import numpy as np
from time import sleep

p = figure(title="X vs Random Number Generator", plot_height=350, plot_width=800)
target = show(p, notebook_handle=True)

x, y = [], []

for i in range(100):
    x,y,t = get_traces()
    p.line(t, x)
    push_notebook(handle=target)
    sleep(0.001)
```

It seems that if I use `ColumnDataSource` that if I update the dict, it should trigger a replot? 

https://stackoverflow.com/questions/34970704/what-is-a-fast-and-proper-way-to-refresh-update-plots-in-bokeh-0-11-server-app

But it doesn't. And I don't know why.

```python
from bokeh.models import ColumnDataSource

p = figure(title="X vs Random Number Generator", plot_height=350, plot_width=800)
x,y,t = get_traces()
source1 = ColumnDataSource(data=dict(x=t, y=x))
source2 = ColumnDataSource(data=dict(x=t, y=x))
p.line('x', 'y', source=source1)
p.line('x', 'y', source=source2)
target = show(p, notebook_handle=True)

while True:
    x,y,t = get_traces()
    source1.data = dict(x=t, y=x)
    source2.data = dict(x=t, y=y)
    push_notebook(handle=target)
    sleep(0.001)
```

```python
from bokeh.models import ColumnDataSource

p = figure(title="X vs Random Number Generator", plot_height=600, plot_width=600)
x,y,t = get_traces()
source1 = ColumnDataSource(data=dict(x=x, y=y))
p.line('x', 'y', source=source1)
target = show(p, notebook_handle=True)

while True:
    x,y,t = get_traces()
    source1.data = dict(x=x, y=y)
    push_notebook(handle=target)
    sleep(0.001)
```

```python

```
