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

# Interact
Here is a simple example of using the `interact` decorator from ipywidgets to create a simple set of widgets to control the parameters of a plot

```python
import plotly.graph_objs as go
import numpy as np
from ipywidgets import interact
```

First we'll create an empty figure, and add an empty scatter trace to it.

```python
fig = go.FigureWidget()

fig.update_layout(
    margin=dict(l=20, r=20, t=20, b=20),
    width=600,
    height=400,
    template='plotly_white'
)

fig.add_scatter()
scatt = fig.data[0]

xs=np.linspace(0, 6, 1000)

def update(a=3.6, b=4.3, color='blue'):
    with fig.batch_update():
        scatt.y=np.sin(a*xs-b)
        scatt.line.color=color
        
interact(update, a=(1.0, 4.0, 0.01), b=(0, 10.0, 0.01), color=['red', 'green', 'blue'])

fig
```

# Harmonic Oscillator

$$
A(\omega) = \frac{F_0/m}{\sqrt{(2\omega_0\omega\zeta)^2+(\omega^2-\omega_0^2)^2}}
$$

$$
\phi(\omega) = \arctan \left( \frac{2\omega \omega_0 \zeta}{\omega^2-\omega_0^2} \right)
$$

```python
import plotly.graph_objs as go
from plotly.subplots import make_subplots



def amp(w, zeta):
    return 1.0 / np.sqrt((2*zeta*w)**2 + (w**2-1)**2)

def phase(w, zeta):
    return np.arctan2(2*w*zeta,w**2-1)

fig = go.FigureWidget(make_subplots(rows=1, cols=2))
fig.update_layout(height=300, width=900,template='plotly_white', margin=dict(l=20, r=20, t=20, b=20))

fig.add_scatter(row=1,col=1)
fig.add_scatter(row=1,col=2)

fig.data[0].x = w
fig.data[1].x = w

def update(zeta=-2.0):
    zeta = 10**zeta
    with fig.batch_update():
        fig.data[0].y = amp(w,zeta)
        fig.data[1].y = phase(w,zeta)

interact(update,zeta_log=(-3.1,3.1,0.01))
fig
```

# Heat map

```python
x = np.linspace(-5,5,200)
X,Y = np.meshgrid(x,x)
Z = np.exp(-(X**2+Y**2))

fig=go.FigureWidget(go.Heatmap(z=Z, showscale=False))
fig.update_layout(height=400, width=400,template='plotly_white', margin=dict(l=20, r=20, t=20, b=20))

def update(l=1.0):
    with fig.batch_update():
        fig.data[0].z = np.exp(-(X**2+Y**2)/l)

interact(update,l=(0.1,3))
fig
```

With 200x200 images, it becomes noticeably slow. So, it is still not a full replacement for spyview. 

But: I think I can probably do a good linecut explorer:

```python
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import numpy as np
from ipywidgets import interact

x = np.linspace(-5,5,200)
X,Y = np.meshgrid(x,x)
Z = np.exp(-((X-Y/2)**2+Y**2))

fig = go.FigureWidget(make_subplots(rows=1, cols=2))

fig.add_heatmap(z=Z,row=1,col=1,showscale=False)
fig.add_scatter(row=1,col=1,line=dict(color='white',width=1,dash='dash'))
fig.add_scatter(row=1,col=2,line=dict(color='black'))

fig.data[1].x = np.linspace(0,len(x))
fig.data[2].x = x
fig.data[2].y = Z[:,0]

fig.update_layout(height=400, width=950,template='plotly_white', margin=dict(l=20, r=20, t=20, b=20))

def update(i=0):
    with fig.batch_update():
        fig.data[2].y = Z[i,:]
        fig.data[1].y = i*np.ones(len(x))

interact(update,i=(0,len(x)-1))
fig
```

<!-- #region -->
# Links


https://plot.ly/python/setting-graph-size/

https://plot.ly/python/templates/

<!-- #endregion -->
