---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.6
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python
import numpy as np
from ipywidgets import interact
import matplotlib.pyplot as plt

from bokeh.models import Range1d, ColorBar, LinearColorMapper
from bokeh.plotting import figure, show
from bokeh.io import output_notebook, push_notebook

import colorcet as cc

# For if you have no internet connection, likeon the plane...
#from bokeh.resources import INLINE
#output_notebook(INLINE)

# This will import the latest compatible bokeh JS from the CDN
output_notebook()
```

# Symmetric SQUID 

```python
phi = np.linspace(-2,2,100)

def EJ(flux):
    return 0.5*(1-np.cos(np.pi*flux)*np.cos(np.pi*phi))

p = figure(height=300, width=600)
p.y_range = Range1d(-0.1,1.1)
p.xaxis.axis_label = "Phase phi"
p.yaxis.axis_label = "E(phi)"
p.sizing_mode = "scale_width"
l = p.line(phi, EJ(0))
target = show(p, notebook_handle=True)

def update_plot(flux=0):
    l.data_source.data = dict(x=phi, y=EJ(flux))
    push_notebook(handle=target)
    
interact(update_plot, flux=(0,1,0.01))
```

Actually, pretty cool to see here: when you switch flux, you also jump in  equilibrium position in phase. 

Let's add a 2D colorplot:

```python
phi = np.linspace(-2,2,200)
flux = np.linspace(-2,2,200)

phi,flux = np.meshgrid(phi,flux)

E = 0.5*(1-np.cos(np.pi*flux)*np.cos(np.pi*phi))

p = figure(height=400, width=500) 
p.grid.visible = False
p.x_range.range_padding = p.y_range.range_padding = 0
pal = cc.CET_D1A
pal = cc.coolwarm
#pal = cc.blues
#pal = cc.kbc
pal = cc.rainbow4
color_mapper = LinearColorMapper(palette=pal)
#color_mapper.low = -np.max(np.abs(E))
#color_mapper.high = -color_mapper.low
im = p.image(image=[E],x=0, y=0, dw=1, dh=1, color_mapper = color_mapper)
p.add_layout(ColorBar(color_mapper = color_mapper), 'right')
show(p)
```

# Asymmetric SQUID

```python
phi = np.linspace(-2,2,100)

def EJ(flux, r):
    E = 0.5
    E -= 0.5*(1-r)*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E -= 0.5*(1-r)*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    E -= 0.5*r*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E += 0.5*r*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    return E

p = figure(height=300, width=600)
p.y_range = Range1d(-0.1,1.1)
p.xaxis.axis_label = "Phase"
p.yaxis.axis_label = "E(Phase)"
p.sizing_mode = "scale_width"
l = p.line(phi, EJ(0,0.5))
target = show(p, notebook_handle=True)

def update_plot(flux=0, r=0.5):
    l.data_source.data = dict(x=phi, y=EJ(flux, r))
    push_notebook(handle=target)
    
interact(update_plot, flux=(-2,2,0.01), r=(0,1,0.01))
```

```python
phi = np.linspace(-2,2,200)
flux = np.linspace(-2,2,200)

phi,flux = np.meshgrid(phi,flux)

def E_ind(phi,flux,r):
    E = 0.5
    E -= 0.5*(1-r)*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E -= 0.5*(1-r)*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    E -= 0.5*r*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E += 0.5*r*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    return E

E = E_ind(phi,flux,0.25)

p = figure(height=400, width=500) 
p.grid.visible = False
p.x_range.range_padding = p.y_range.range_padding = 0
pal = cc.rainbow4
color_mapper = LinearColorMapper(palette=pal)
#color_mapper.low = -np.max(np.abs(E))
#color_mapper.high = -color_mapper.low
im = p.image(image=[E],x=0, y=0, dw=1, dh=1, color_mapper = color_mapper)
p.add_layout(ColorBar(color_mapper = color_mapper), 'right')

handle = show(p, notebook_handle=True)

def update_plot(r=0.25):
    im.data_source.data['image'] = [E_ind(phi,flux,r)]
    push_notebook(handle=handle)
    
interact(update_plot, r=(0,1,0.01))
```

Let's check out the third and fourth order anharmonicities:

```python
phi = np.linspace(-2,2,200)
flux = np.linspace(-2,2,200)

phi,flux = np.meshgrid(phi,flux)

def E_ind(r):
    E = 0.5
    E -= 0.5*(1-r)*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E -= 0.5*(1-r)*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    E -= 0.5*r*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E += 0.5*r*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    return E

def Z_func(r):
    E = E_ind(r)
    return np.diff(E,n=4,axis=1)

Z = Z_func(0.25)

p = figure(height=400, width=500) 
p.grid.visible = False
p.x_range.range_padding = p.y_range.range_padding = 0
pal = cc.rainbow4
color_mapper = LinearColorMapper(palette=pal)
#color_mapper.low = -np.max(np.abs(E))
#color_mapper.high = -color_mapper.low
im = p.image(image=[Z],x=0, y=0, dw=1, dh=1, color_mapper = color_mapper)
p.add_layout(ColorBar(color_mapper = color_mapper), 'right')

handle = show(p, notebook_handle=True)

def update_plot(r=0.25):
    im.data_source.data['image'] = [Z_func(r)]
    push_notebook(handle=handle)
    
interact(update_plot, r=(0,1,0.01))
```

Ah, of course, we have to be careful! We care only about the higher order derivatives at the position of the minima. 

```python
phi = np.linspace(-2,2,100)

def E(flux, r):
    E = 0.5
    E -= 0.5*(1-r)*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E -= 0.5*(1-r)*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    E -= 0.5*r*np.cos(np.pi*phi)*np.cos(np.pi*flux)
    E += 0.5*r*np.sin(np.pi*phi)*np.sin(np.pi*flux)
    return E

def diffs(x,y):
    yd1 = np.diff(y,n=1)
    x1 = (x[0:-1]+x[1:])/2
    yd2 = np.diff(y,n=2)
    x2 = x[1:-1]
    yd3 = np.diff(y,n=3)
    x3 = (x[2:-1]+x[1:-2])/2
    yd4 = np.diff(y,n=4)
    x4 = x[2:-2]
    dx = x[1]-x[0]
    return x1,yd1/dx**0.5,x2,yd2/dx,x3,yd3/dx**1.5,x4,yd4/dx**2

p = figure(height=300, width=600)
p.y_range = Range1d(-1.1,1.1)
p.xaxis.axis_label = "Phase"
p.yaxis.axis_label = "E(Phase)"
p.sizing_mode = "scale_width"
x = phi
y = E(0,0.5)
l = p.line(x, y)
x1,yd1,x2,yd2,x3,yd3,x4,yd4 = diffs(x,y)
l1 = p.line(x1,yd1, line_color='black') # slope is black (zeros are minima)
l2 = p.line(x2,yd2, line_color="blue") # stiffness is blue
l3 = p.line(x3,yd3, line_color='red') # Cubic is red
l4 = p.line(x4,yd4, line_color='green') # Kerr is green
target = show(p, notebook_handle=True)
print("Black = Slope (zero = minimum), Blue = inverse stiffness, Red = Cubic term, Green = Kerr")


def update_plot(flux=0, r=0.5, hide=0):
    x = phi
    y = E(flux,r)
    x1,yd1,x2,yd2,x3,yd3,x4,yd4 = diffs(x,y)
    l.data_source.data = dict(x=x, y=y)
    l1.data_source.data = dict(x=x1,y=yd1)
    l2.data_source.data = dict(x=x2,y=yd2)
    l3.data_source.data = dict(x=x3,y=yd3)
    l4.data_source.data = dict(x=x4, y=yd4)
    for line in l1,l2,l3,l4:
        line.visible = not bool(hide)
    push_notebook(handle=target)
    
interact(update_plot, flux=(-2,2,0.01), r=(0,1,0.01), hide=(0,1,1))
```

# RF SQUID (ideal SNAIL?)

```python
phi = np.linspace(-3,3,100)

def E(flux, r):
    E = (phi+flux)**2
    E += r*(1-np.cos(np.pi*(phi-flux)))
    return E

p = figure(height=600, width=800)
p.y_range = Range1d(-1.1,5.1)

def diffs(x,y):
    yd1 = np.diff(y,n=1)
    x1 = (x[0:-1]+x[1:])/2
    yd1 = yd1/np.max(yd1)
    yd2 = np.diff(y,n=2)
    x2 = x[1:-1]
    yd2 = yd2/np.max(yd2)
    yd3 = np.diff(y,n=3)
    yd3 = yd3/np.max(yd3)
    x3 = (x[2:-1]+x[1:-2])/2
    yd4 = np.diff(y,n=4)
    yd4 = yd4/np.max(yd4)
    x4 = x[2:-2]
    return x1,yd1,x2,yd2,x3,yd3,x4,yd4

p.xaxis.axis_label = "Phase"
p.yaxis.axis_label = "E(Phase)"
p.sizing_mode = "scale_width"

x = phi
y = E(0,0.5)
l = p.line(x, y)
x1,yd1,x2,yd2,x3,yd3,x4,yd4 = diffs(x,y)
l1 = p.line(x1,yd1, line_color='black') # slope is black (zeros are minima)
l2 = p.line(x2,yd2, line_color="blue") # stiffness is blue
l3 = p.line(x3,yd3, line_color='red') # Cubic is red
l4 = p.line(x4,yd4, line_color='green') # Kerr is green
target = show(p, notebook_handle=True)
print("All normalised!\nBlack = Slope (zero = minimum), Blue = inverse stiffness, Red = Cubic term, Green = Kerr")

def update_plot(flux=0, r=0.5, hide=1):
    x = phi
    y = E(flux,r)
    x1,yd1,x2,yd2,x3,yd3,x4,yd4 = diffs(x,y)
    l.data_source.data = dict(x=x, y=y)
    l1.data_source.data = dict(x=x1,y=yd1)
    l2.data_source.data = dict(x=x2,y=yd2)
    l3.data_source.data = dict(x=x3,y=yd3)
    l4.data_source.data = dict(x=x4, y=yd4)
    for line in l1,l2,l3,l4:
        line.visible = not bool(hide)
    push_notebook(handle=target)
    
interact(update_plot, flux=(-2,2,0.01), r=(0,1,0.01), hide=(0,1,1))
```

OK, that is totally the way to look at it! You can see the flux qubit, and also the snail. 


# Asymmetrically threaded SQUID (ATS)

```python
phi = np.linspace(-3,3,100)

def E(flux_sum, flux_diff, r, delta):
    E = (phi)**2
    E += -2*r*np.cos(np.pi*flux_sum)*np.cos(np.pi*(phi+flux_diff))
    E += 2*r*delta*np.sin(np.pi*flux_sum)*np.sin(np.pi*(phi+flux_diff))
    return E
```

```python
p = figure(height=600, width=800)
p.y_range = Range1d(-1.1,5.1)

def diffs(x,y):
    yd1 = np.diff(y,n=1)
    x1 = (x[0:-1]+x[1:])/2
    #yd1 = yd1/np.max(yd1)
    yd2 = np.diff(y,n=2)
    x2 = x[1:-1]
    #yd2 = yd2/np.max(yd2)
    yd3 = np.diff(y,n=3)
    #yd3 = yd3/np.max(yd3)
    x3 = (x[2:-1]+x[1:-2])/2
    yd4 = np.diff(y,n=4)
    #yd4 = yd4/np.max(yd4)
    x4 = x[2:-2]
    #return x1,yd1,x2,yd2,x3,yd3,x4,yd4
    dx = x[1]-x[0]
    return x1,yd1/dx**0.5,x2,yd2/dx,x3,yd3/dx**1.5,x4,yd4/dx**2


p.xaxis.axis_label = "Phase"
p.yaxis.axis_label = "E(Phase)"
p.sizing_mode = "scale_width"

x = phi
y = E(0,0.5, 0.1, 0)
l = p.line(x, y)
x1,yd1,x2,yd2,x3,yd3,x4,yd4 = diffs(x,y)
l1 = p.line(x1,yd1, line_color='black') # slope is black (zeros are minima)
l2 = p.line(x2,yd2, line_color="blue") # stiffness is blue
l3 = p.line(x3,yd3, line_color='red') # Cubic is red
l4 = p.line(x4,yd4, line_color='green') # Kerr is green
target = show(p, notebook_handle=True)
print("All normalised!\nBlack = Slope (zero = minimum), Blue = inverse stiffness, Red = Cubic term, Green = Kerr")

def update_plot(flux_sum=0, flux_diff=0, delta=0, r=0.5, hide=1):
    x = phi
    y = E(flux_sum, flux_diff, r, delta)
    x1,yd1,x2,yd2,x3,yd3,x4,yd4 = diffs(x,y)
    l.data_source.data = dict(x=x, y=y)
    l1.data_source.data = dict(x=x1,y=yd1)
    l2.data_source.data = dict(x=x2,y=yd2)
    l3.data_source.data = dict(x=x3,y=yd3)
    l4.data_source.data = dict(x=x4, y=yd4)
    for line in l1,l2,l3,l4:
        line.visible = not bool(hide)
    push_notebook(handle=target)
    
# flux_sum, flux_diff, r, delta
interact(update_plot, 
         flux_sum=(-2,2,0.01), 
         flux_diff=(-2,2,0.01),
         delta = (0,1,0.01),
         r=(0,1,0.01), 
         hide=(0,1,1))
```

```python

```

```python

```
