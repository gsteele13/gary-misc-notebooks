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

```python hide_input=false
import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit
from ipywidgets import interact
```

```python hide_input=false
plt.rcParams['figure.dpi'] = 100
```

# Interactively exploring $\chi^2$


## Model $y=a$ 

```python hide_input=false
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

## Model $y = ax$

```python hide_input=true
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

#  Fitting


## Model  $y = a$

Goal: 

* Genereate a dataset with error from a model
* Fit the model with `curve_fit` and extract parameter errors
* Take a look at the fitted value and the error values a plot of $\chi^2$


```python hide_input=false
def make_plots(N):
    # Generate data
    x = np.linspace(0,1,N)
    y = np.random.normal(size=len(x))*2+2

    # A sweep of model parameter and calculating chi2

    Npar = 100
    a_range = 3
    a_val = 2
    a1 = a_val-a_range/2
    a2 = a_val+a_range/2
    a = np.linspace(a1, a2,Npar)

    for i in range(len(a)):
        chi2[i] = np.sum((a[i]-y)**2)/len(y)

    # Now perform a least squares fit

    def f(x,a):
        return a

    val,cov = curve_fit(f,x,y)

    a_fit = val[0]
    a_err = np.sqrt(cov[0,0])

    print("Fitted value of a", a_fit)
    print("Error on a:", a_err)

    plt.subplots(figsize=(12,4))
    plt.subplot(121)
    plt.plot(x,y)
    plt.plot(x,a_fit*np.ones(len(x)))
    plt.subplot(122)
    plt.plot(a,chi2,'.-')
    plt.axvline(a_fit, ls=':', c='black')
    plt.axvline(a_fit-a_err, ls=':', c='grey')
    plt.axvline(a_fit+a_err, ls=':', c='grey')
    plt.axvline(a_fit-2*a_err, ls=':', c='grey')
    plt.axvline(a_fit+2*a_err, ls=':', c='grey')
    plt.show()
```

```python
make_plots(100)
```

```python
make_plots(1000)
```

# Error vs number of points

Does it give what you would expect? 

```python hide_input=false
# Generate data
Nmax = 1e6
x = np.linspace(0,1,int(Nmax))
y = np.random.normal(size=len(x))*2+2

N = np.geomspace(2,Nmax,100).astype(int)

val = np.zeros(len(N))
err = np.zeros(len(N))

def f(x,a):
    return a

for i in range(len(N)):
    n = N[i]
    v,e = curve_fit(f,x[0:n],y[0:n])
    val[i] = v[0]
    err[i] = np.sqrt(e[0,0])

plt.plot(N,val,)
plt.plot(N,err,)
plt.xscale('log')
plt.yscale('log')

plt.grid()
```

<!-- #region {"hide_input": true} -->
# Fitting filtered data: "Moving average" vs. "Bin average"

Is this fair? 
<!-- #endregion -->

```python hide_input=false
# Generate data
Nmax = 1000
x = np.linspace(0,1,int(Nmax))
y = np.random.normal(size=len(x))*2+2

Nf = 50

def moving_average(x, N):
    cumsum = np.cumsum(np.insert(x, 0, 0)) 
    return (cumsum[N:] - cumsum[:-N]) / float(N)

x_ma  = moving_average(x,Nf)
y_ma = moving_average(y,Nf)

def bin_average(x, N):
    x2 = x[0:len(x)//N*N]
    x2 = np.reshape(x2,[len(x2)//N,N])
    return np.average(x2,axis=1)

x_ba = bin_average(x,Nf)
y_ba = bin_average(y,Nf)

plt.figure(figsize=(12,6))
plt.plot(x,y)
plt.plot(x_ma,y_ma)
plt.plot(x_ba,y_ba,'o',c='yellow')
plt.show()

val,cov = curve_fit(f,x,y)
val_ma,cov_ma = curve_fit(f,x_ma,y_ma)
val_ba,cov_ba = curve_fit(f,x_ba,y_ba)

dec =  3
print("Val   ", round(val[0],dec))
print("Val MA", round(val_ma[0],dec))
print("Val BA", round(val_ba[0],dec))
print()
print("Err   ", round(np.sqrt(cov[0,0]),dec))
print("Err MA", round(np.sqrt(cov_ma[0,0]),dec))
print("Err BA", round(np.sqrt(cov_ba[0,0]),dec))
```

Naughty cheater!
