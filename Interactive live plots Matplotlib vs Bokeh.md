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

# Matplotlib Widget

```python
import numpy as np
import matplotlib.pyplot as plt
import time
```

```python
%matplotlib widget

x = np.linspace(-10,10,100)
def f(x):
    return np.sin(x)+np.random.normal(size=len(x))*0.1

fig, ax = plt.subplots(figsize=(4,3))
line, = ax.plot(x,0*x)
ax.set_ylim(-1.5,1.5)
line.set_data(x,f(x))
plt.show()
```

```python
t0 = time.time()

N = 100
for i in range(N):
    line.set_data(x,f(x))
    fig.canvas.draw()
print()
t = time.time()-t0

print("Plots per second: ", (N/t))
```

Fundamental problem: interaction is broken during live updates. Really a killer. 


# Bokeh

```python
import numpy as np
import matplotlib.pyplot as plt
import time

from bokeh.plotting import figure, show
from bokeh.io import output_notebook, push_notebook
from bokeh.models import ColumnDataSource, Toggle, Range1d

output_notebook()
```

```python

x = np.linspace(-10,10,100)
def f(x):
    return np.sin(x)+np.random.normal(size=len(x))*0.1

source = ColumnDataSource()
source.data = dict(x=x, y=f(x))

p = figure(title="test", plot_height=300, plot_width=500)
l = p.line('x', 'y', source=source)
target = show(p, notebook_handle=True)

N = 1000
t0 = time.time()
for i in range(N):
    l.data_source.data['y'] = f(x)
    push_notebook()
t = time.time()-t0
```

```python
print("Plots per second: ", (N/t))
```

OK, there is not really any competition at all. Bokeh is 20 times faster and preserves interactions during plotting. Really a no brainer. 
