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

omega = 1.6

def force(x, v, t):
    f = x - x**3 + 2.0*np.cos(2.4*t)
    return f

def verlet_steps(x, v, h, steps):
    gamma = 0.2
    t = 0
    fac_pl = 1.0/(1 + 0.5*gamma*h)
    fac_mi = 1 - 0.5*gamma*h
    f = force(x, v, t)
    for i in range(steps):
        v = fac_mi*v + 0.5*h*f # From now until four lines down, v is evaluated at the half time step.
        x = x + h*v
        t = t + h
        f = force(x, v, t)
        v = (v + 0.5*h*f)*fac_pl
    return x, v
Ni = 200
steps = 2000
h = 0.02
x_vals = np.linspace(-6,6,Ni)
v_vals = np.linspace(-6,6,Ni)

X, V = np.meshgrid(x_vals, v_vals)
X, V = verlet_steps(X,V, h, steps)
```

Note quite what I was looking for...
