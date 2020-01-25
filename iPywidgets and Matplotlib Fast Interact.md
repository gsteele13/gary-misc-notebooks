---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.3.0
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

%matplotlib notebook

x = np.linspace(-10,10,100)

fig, ax = plt.subplots(figsize=(4,3))
line, = ax.plot(x,np.sin(x))

def makeplot2(ph = 0):
    line.set_data(x,np.sin(x+ph))

interact(makeplot2, ph=(0,10,0.1))
```

```python
x = np.linspace(-3,3,1000)
X,Y = np.meshgrid(x,x)
x0 = 0
Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
ax_image = plt.imshow(Z,cmap='RdBu')

def makeplot3(x0 = 0):
    Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
    ax_image.set_data(Z)

interact(makeplot3,x0=(-1,1,0.1))

```

```python

```
