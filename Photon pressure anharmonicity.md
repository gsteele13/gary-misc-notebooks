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
import matplotlib.pyplot as plt
import numpy as np
from qutip import *
```

# Photon pressure anhamonicity

Let's try to use QuTiP to calculate the anharmonicity you get in photon-pressure cQED. 


## First, single HO

```python
N = 20
a = destroy(N)

w1 = 5 

H = w1*a.dag()*a 
plt.plot(H.eigenenergies(), '.')
```

## Next, transmon

```python
N = 10
a = destroy(N)

w1 = 5
A1 = -0.3/2

#H = w1*a.dag()*a + A1*(a+a.dag())**4
H = w1*a.dag()*a + A1*a.dag()*a.dag()*a*a

E = H.eigenenergies()

anharom = (E[1]-E[0])-(E[2]-E[1])
print(anharom)

plt.plot(E, 'o-')
plt.show()
```

## First, dipole cQED between transmon and cavity

```python
N = 20
a = tensor(destroy(N),qeye(N))
b = tensor(qeye(N),destroy(N))

w1 = 5
w2 = 8
g = 0.1

H = w1*a.dag()*a + w2*b.dag()*b
```

```python
plt.plot(H.eigenenergies(), '.')
```

```python
H = 
```
