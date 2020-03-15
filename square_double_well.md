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
import scipy.linalg as LA
from ipywidgets import interact
```

```python
plt.rcParams['figure.dpi'] = 100
```

```python
N = 101

# Our kinetic energy
T = 2*np.eye(N)-np.eye(N,k=1)-np.eye(N,k=-1)

# Our potential energy

def update(v0=0):
    v0 *= 1e-3
    x = np.array(range(N))
    v = v0*(np.abs(x+0.5-N/2) <= N/10)
    V = np.diag(v,0)
    H = T + V
    val,vec = LA.eigh(H)
    if (vec[20,1] < 0):
        vec[:,1] *= -1
        
    plt.figure(figsize=(12,4))
    plt.subplot(121)
    for i in range(2):
        plt.plot(vec[:,i])
    plt.axhline(0, ls=":", c='grey')
    plt.title(r"Energy diffence = %.2f $\times 10^{-4}$" % ((val[1]-val[0])*1e4))
    plt.subplot(122)
    plt.plot(v, 'k')
    cycle = plt.rcParams['axes.prop_cycle'].by_key()['color']
    for i in range(2):
        plt.plot(vec[:,i]/100+val[i])
        plt.axhline(val[i], ls=':', c=cycle[i])
    plt.ylim((0,8e-3))
    plt.title(r"v0 = %.0f $\times 10^{-3}$" % (v0*1e3))
    
    #print("Eigenvalues:\n %e %e\nDiff\n%e" % (w[0], w[1], w[1]-w[0]))
    plt.show()
    
interact(update, v0=(0,50, 1))
```

```python

```
