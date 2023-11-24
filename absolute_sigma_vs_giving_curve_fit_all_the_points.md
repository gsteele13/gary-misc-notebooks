---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.5
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python
import numpy as np
import matplotlib.pyplot as plt
```

# Formulation  of the problem

Say I have a set of 10 repeated measurements for 5 different experimental settings. 

How should I fit this data?  

There are two  approaches I can think of that should be equivalent:

1. Use `curve_fit` with all the data, providing no sigma (assumption  of equal vertical error on each point)
2. For each x-value, calculate the average y value and the standard deviation. Feed this then into curve_fit as the error value using `sigma=` and then also specify that these represent the absolute error using `absolute_sigma=True`

On first glance, I thought that these would be equivalent. But my experiments below suggest it is not...?


## Generate the data

To make sure I avoid the problems of convergence for small sets of numbers, I will make the generated data with 100 repetitions for each x-value.

```python
N = 5 # x values
M =  100 # repeat
x  = np.array(list(range(N))*M)
y = 5*x + np.random.normal(size=N*M)
plt.plot(x,y,'o')
```

## Technique 1:  Fit without absolute error

```python
from scipy.optimize import curve_fit
```

```python
def f(x,a,b):
    return a*x + b

val,cov = curve_fit(f,x,y)

print(val[0])
print(val[1])
print()
print(np.sqrt(cov[0,0]))
print(np.sqrt(cov[1,1]))
```

##  Technique 2: Calculate standard deviation and use that as absolute error

```python
xs = np.reshape(x,(M,N))
#xs
```

```python
xavg = np.average(xs,axis=0)
xavg
```

```python
ys = np.reshape(y,(M,N))
yavg = np.average(ys,axis=0)
yerr = np.std(ys,axis=0)
```

```python
plt.errorbar(xavg,yavg,yerr,marker='o')
```

```python
val,cov = curve_fit(f,xavg,yavg,sigma=yerr, absolute_sigma=True)

print(val[0])
print(val[1])
print()
print(np.sqrt(cov[0,0]))
print(np.sqrt(cov[1,1]))
```

OK, crazy, why are these so different? A factor of 10!?

Hmmm...if I increase the number of "repeats", then it gets EVEN worse!

I guess I am missing an important point: the error I am feeding into the fit should not be the standard deviation, since that is the error of a single point, and not of the average. 

The error on the average should be reduced by a factor of $\sqrt{M}$...that must be it! 

Let's check.

```python
val,cov = curve_fit(f,xavg,yavg,sigma=yerr/np.sqrt(M), absolute_sigma=True)

print(val[0])
print(val[1])
print()
print(np.sqrt(cov[0,0]))
print(np.sqrt(cov[1,1]))
```

**Oooooooo, yeah!**
