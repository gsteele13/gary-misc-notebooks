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
import scipy.linalg as LA
```

```python
plt.rcParams['figure.dpi'] = 100
```

```python
N = 1000
H = 2*np.eye(N)-np.eye(N,k=1)-np.eye(N,k=-1)
w,v = LA.eigh(H)
plt.figure(figsize=(8,10))
plt.subplot(121)
for i in range(5):
    e = w[i]
    plt.plot(v[:,i]/N/10+e)
    plt.axhline(e, ls=":", c='grey')
plt.subplot(122)
i=np.array(range(N))+1
plt.plot(i,w)
plt.plot(i,i**2/N**2*10)
plt.show()
```

```python

```
