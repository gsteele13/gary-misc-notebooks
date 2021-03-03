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

```python
# Flux quantum
Phi0 = h/2/e

# Handy
def fmt(val, unit):
    return "<td>" + EngFormatter(places=1)(val) + unit
```

# Critical current, Josephson Inductance, Josephson Energy

https://en.wikipedia.org/wiki/Josephson_effect#Josephson_inductance

```python
html = "<table><th>Ic <th> L_J <th> E_J/h" 
for a in range(-13,-4):
    for b in (1,2,5):
        Ic = b*10**(a)
        Lj = Phi0/2/pi/Ic
        Ej = Phi0*Ic/2/pi
        html += "<tr>"
        html += fmt(Ic, "A")
        html += fmt(Lj, "H")
        html += fmt(Ej/h, "Hz")
html += "</table>"
HTML(html)
```

# Temperature to frequency

```python
html = "<table><th> T <th> kT/h" 
for a in range(-6,2):
    for b in (1,2,5):
        T = b*10**a
        kT = k*T/h
        html += "<tr>"
        html += fmt(T, "K")
        html += fmt(kT, "Hz")
html += "</table>"
HTML(html)
```
