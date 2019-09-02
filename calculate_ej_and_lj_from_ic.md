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

$$
E_J = \frac{\hbar I_c}{2e} 
$$

in Joules.

Energy to GHz:

$$
E = h \nu
$$

Josephson inductance:

$$
L_J = \frac{\hbar}{2 e I_c}
$$

```python
from scipy.constants import *
```

```python
print("%.1f" % (hbar*1e-6/2/e/h/1e9))
```

```python
def calc_EJ_LJ(Ic):
    EJ = (hbar*Ic/2/e/h/1e9) # In GHz
    LJ = (hbar/2/e/Ic/1e-9) # In nH
    print("Ic % 7.3f uA   Ej % 7.1f GHz   LJ % 7.2f nH" % (Ic/1e-6, EJ, LJ) )

for i in [9,8,7,6,5]:
    calc_EJ_LJ(1*10**(-i))
```

```python
calc_EJ_LJ(1e-6)
```

```python
calc_EJ_LJ(0.1e-6)
```

```python
calc_EJ_LJ(0.2e-6)
```

```python
calc_EJ_LJ(0.4e-6)
```

```python

```

```python

```
