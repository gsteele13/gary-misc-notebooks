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
```

```python
plt.rcParams['figure.dpi'] = 100
```

# Frequency of a harmonic oscillator

Here, we will plot the frequency of a harmonic oscillator as a function of the spring constant.

## Equations

From our equations, we know that:

$$
\omega = \sqrt{\frac{k}{m}}
$$

Let's plot it!

```python
k = np.linspace(0,10,100)
m = 1
omega = np.sqrt(k/m)
plt.plot(k,omega)
plt.xlabel("Spring constant (N/m)")
plt.ylabel("Angular frequency $\omega$ (rad/s)")
```
