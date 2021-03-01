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
from scipy.constants import *
import numpy as np
from IPython.display import HTML
from matplotlib.ticker import EngFormatter

```

https://en.wikipedia.org/wiki/Josephson_effect#Josephson_inductance

```python
Phi0 = h/2/e

def LJ(Ic):
    return Phi0/2/pi/Ic

def EJ(Ic):
    return Phi0*Ic/2/pi
```

```python
html = "<table><th>Ic (nA) <th> L_J (nH) <th> E_J (GHz)" 
f = EngFormatter(places=1)
for a in range(-1,4):
    for b in (1,2,5,10):
        Ic = b*10**a*1e-9
        html += "<tr>"
        html += "<td>" + f"{f(Ic)}"
        html += "<td>" + f"{f(LJ(Ic))}"
        html += "<td>" + f"{f(EJ(Ic)/h)}"
html += "</table>"
HTML(html)
```
