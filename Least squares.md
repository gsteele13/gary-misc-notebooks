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
from  ipywidgets  import interact
```

```python
plt.rcParams['figure.dpi'] = 100
```

Model: 

$$
y = b
$$

```python
# Generate data

N = 100
x = np.linspace(0,1,N)
y = np.random.normal(size=len(x))

# A sweep of model parameter and calculating chi2

Npar = 100
b_max = 1
b = np.linspace(-b_max, b_max,Npar)

chi2 = np.zeros(len(b))

# this could be vectorized, but in a loop here for clarity
for i in range(len(b)):
    chi2[i] = np.sum((b[i]-y)**2)/len(y)

# Now interactive plots

def update(i=0):
    plt.subplots(figsize=(12,4))
    plt.subplot(121)
    plt.plot(x,y, label="Data")
    plt.plot(x,b[i]*np.ones(len(x)), label="Model $y = b$")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.legend()
    plt.subplot(122)
    plt.plot(b,chi2)
    plt.plot(b[i], chi2[i], 'o')
    plt.xlabel("Model parameter $b$")
    plt.ylabel("$\chi^2$")
    plt.show()
    
interact(update, i=(0,len(b)-1,5));
```

Model: 

$$
y = ax
$$

```python
# Generate data

N = 100
x = np.linspace(0,1,N)
y = 10*x+np.random.normal(size=len(x))*2

## Parameter sweep of parameter a

Npar = 100
a_range = 20
a_val = 10
a1 = a_val-a_range/2
a2 = a_val+a_range/2
a = np.linspace(-a1, a2,Npar)

chi2 = np.zeros(len(a))

for i in range(len(a)):
    chi2[i] = np.sum((a[i]*x-y)**2)/len(y)

# Now interactive plots

def update(i=0):
    plt.subplots(figsize=(12,4))
    plt.subplot(121)
    plt.plot(x,y, label="Data")
    plt.plot(x,a[i]*x, label="Model $y = ax$")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.legend()
    plt.subplot(122)
    plt.plot(a,chi)
    plt.plot(a[i], chi[i], 'o')
    plt.xlabel("Model parameter $a$")
    plt.ylabel("$\chi^2$")
    plt.show()
    
interact(update, i=(0,len(a)-1,5))
```

```python

```
