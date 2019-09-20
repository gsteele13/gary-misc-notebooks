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

```python hide_input=true
import numpy as np
import matplotlib.pyplot as plt
from  ipywidgets  import interact
```

```python hide_input=true
plt.rcParams['figure.dpi'] = 100
```

# Model $y=a$ 

```python hide_input=true
# Generate data

N = 100
x = np.linspace(0,1,N)
y = np.random.normal(size=len(x))+2

# A sweep of model parameter and calculating chi2

Npar = 100
a_range = 3
a_val = 2
a1 = a_val-a_range/2
a2 = a_val+a_range/2
a = np.linspace(a1, a2,Npar)

chi2 = np.zeros(len(a))

# this could be vectorized, but in a loop here for clarity
for i in range(len(a)):
    chi2[i] = np.sum((a[i]-y)**2)/len(y)
    
ym = np.outer(a,np.ones(len(x)))
    
# Now interactive plots

def update(i=0):
    plt.subplots(figsize=(12,7))
    plt.subplot(221)
    plt.plot(x,y, label="Data")
    plt.plot(x,ym[i], label="Model $y = b$")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.legend()
    plt.subplot(222)
    plt.plot(a,chi2)
    plt.plot(a[i], chi2[i], 'o')
    plt.xlabel("Model parameter $b$")
    plt.ylabel("$\chi^2$")
    plt.subplot(223)
    plt.plot(x,y-ym[i])
    plt.axhline(0,ls=':', c='grey')
    plt.xlabel("x")
    plt.ylabel("Residual value")
    plt.subplot(224)
    plt.hist(y-ym[i], bins=50)
    plt.xlim(-10,10)
    plt.axvline(0,ls=':', c='grey')
    plt.xlabel("Residual value")
    plt.ylabel("Histogram count")
    
interact(update, i=(0,len(a)-1,5));
```

# Model $y = ax$

```python hide_input=false
# Generate data

N = 100
x = np.linspace(0,1,N)
y = np.random.normal(size=len(x))+10*x

# A sweep of model parameter and calculating chi2

Npar = 100
a_range = 20
a_val = 10
a1 = a_val-a_range/2
a2 = a_val+a_range/2
a = np.linspace(a1, a2,Npar)

chi2 = np.zeros(len(a))

# this could be vectorized, but in a loop here for clarity
for i in range(len(a)):
    chi2[i] = np.sum((a[i]*x-y)**2)/len(y)
    
ym = np.outer(a,x)
    
# Now interactive plots

def update(i=0):
    plt.subplots(figsize=(12,7))
    plt.subplot(221)
    plt.plot(x,y, label="Data")
    plt.plot(x,ym[i], label="Model $y = b$")
    plt.xlabel("x")
    plt.ylabel("y")
    plt.legend()
    plt.subplot(222)
    plt.plot(a,chi2)
    plt.plot(a[i], chi2[i], 'o')
    plt.xlabel("Model parameter $b$")
    plt.ylabel("$\chi^2$")
    plt.subplot(223)
    plt.plot(x,y-ym[i])
    plt.axhline(0,ls=':', c='grey')
    plt.xlabel("x")
    plt.ylabel("Residual value")
    plt.subplot(224)
    plt.hist(y-ym[i], bins=50)
    plt.xlim(-15,15)
    plt.axvline(0,ls=':', c='grey')
    plt.xlabel("Residual value")
    plt.ylabel("Histogram count")
    
interact(update, i=(0,len(a)-1,5));
```
