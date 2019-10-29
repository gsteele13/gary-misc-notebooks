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

```python
import numpy as np
import matplotlib.pyplot as plt
from ipywidgets import interact
```

```python
x = np.linspace(0,10,100)
```

```python
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
x = np.linspace(-3,3,100)
X,Y = np.meshgrid(x,x)
x0 = 0
Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
ax_image = plt.imshow(Z,cmap='RdBu')

def makeplot3(x0 = 0):
    Z = np.sin(5*(X-x0))*np.exp(-X**2-Y**2)
    ax_image.set_data(Z)

interact(makeplot3,x0=(-5,5,0.1))

```

```python

```
