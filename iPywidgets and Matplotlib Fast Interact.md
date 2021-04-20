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
from ipywidgets import interact
```

```python
%matplotlib inline

x = np.linspace(0,10,100)

def makeplot1(ph = 0):
    plt.plot(x,np.sin(x+ph))
    
interact(makeplot1, ph=(0,10,0.1))
```

```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact

%matplotlib widget

x = np.linspace(-10,10,100)

fig, ax = plt.subplots(figsize=(4,3))
line, = ax.plot(x,np.sin(x))

def makeplot2(ph = 0):
    line.set_data(x,np.sin(x+ph))

interact(makeplot2, ph=(0,10,0.1))
```

```python
%matplotlib widget
```

```python
x = np.linspace(-3,3,200)
X,Y = np.meshgrid(x,x)
x0 = 0
plt.figure()
Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
plt.subplot(121)
ax_image = plt.imshow(Z,cmap='RdBu')
plt.subplot(122)
plt.plot(Z[0:,])

def makeplot3(x0 = 0):
    Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
    ax_image.set_data(Z)

interact(makeplot3,x0=(-1,1,0.1))
```

```python
x = np.linspace(-3,3,500)
X,Y = np.meshgrid(x,x)
x0 = 0
Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
plt.figure()
ax_image = plt.imshow(Z,cmap='RdBu')

def makeplot3(x0 = 0):
    Z = np.exp(-(X-x0)**2-Y**2)
    ax_image.set_data(Z)

interact(makeplot3,x0=(-2,2,0.1))
```

# Live plotting test

```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact
from time import sleep

%matplotlib widget

x = np.linspace(-10,10,100)

fig, ax = plt.subplots(figsize=(4,3))
line, = ax.plot(x,0*x)
ax.set_ylim(-1.5,1.5)
line.set_data(x,np.sin(x)+np.random.normal(size=len(x))*0.1)
plt.show()
```

```python
while True:
    line.set_data(x,np.sin(x)+np.random.normal(size=len(x))*0.1)
    fig.canvas.draw()
    print(".", end="")
```

```python

```
