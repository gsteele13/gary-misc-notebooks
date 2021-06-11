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
k_B = k
```

$$
\omega = \sqrt{\frac{k}{m}}
$$

$$
x_{th}^2 = \frac{k_B T}{k}
$$

Now, we typically know the thickeness / dimensions. 

We will approximate the effective mode mass by the total mass (this will give a correction factor order unity to the displacement, but should give us the order of magnitude OK). 

The density of silicon nitride is nearly the same as that of Al, 2700 kg / m$^3$.

```python
L = 350e-6 # m
t = 100e-9 # m
rho = 2700 # kg/m^3

m = rho*L**2*t
print("Mass: %.2e" % m, "kg", "(%.2f" % (m*1e3/1e-9), "ng)")
```

```python
f = 1e6 # Hz
k = (2*pi*f)**2 * m

print("Spring constant: %.2e" % k, "N/m")
```

```python
x_300K = np.sqrt(k_B * 300 / k)
x_zpf = np.sqrt(h * f / k)

print("Thermal noise amplitude: %.2e" % x_300K)
print("Zero point amplitude: %.2e" % x_zpf)
```
