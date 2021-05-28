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

<!-- #region -->
# Ic to Rn for Aluminum


At T=0, The "Ambegaokar-Baratoff" relation for SIS junctions would predict:

$I_c R_n = \dfrac{\pi \Delta}{2e}$

https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.10.486

For Aluminum, we will take $ \Delta = 173 \mu$V :

https://journals.aps.org/pr/abstract/10.1103/PhysRev.135.A19

(Note, strongly thickeness dependent!)

We then have:

$I_c  = \dfrac{\pi \Delta}{2e R_n}$

We will also add the Ej for the table.
<!-- #endregion -->

```python
Delta = 173e-6 * e # eV / e = Joules
Rn = 100e3
Ic = pi * Delta / 2 / e / Rn
print(Ic)
```

```python hide_input=false
html = "<table><th>Rn <th> Ic <th> L_J <th> E_J/h" 
for a in range(0,6):
    for b in (1,2,5):
        Rn = b*10**a
        Ic = pi * Delta / 2 / e / Rn
        Lj = Phi0/2/pi/Ic
        Ej = Phi0*Ic/2/pi
        html += "<tr>"
        html += fmt(Rn, "Ohms")
        html += fmt(Ic, "A")
        html += fmt(Lj, "H")
        html += fmt(Ej/h, "Hz")
html += "</table>"
HTML(html)
```

```python

```
