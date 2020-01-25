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
```

# Bin averaging

My personally favourite alternative since it produces a reduction of the noise of your data while producing ZERO addition correlations between data points 

**Unlike a low pass filter or a moving average, after which your data points are going to be highly correlated and not statistically independent!**

There seems to be no native nor library function for this, so I've written one here to remind myself how to do it. 


# The code

Here, the function:

```python
def binaverage(x, navg):
    N = len(x) // navg
    return np.average(np.reshape(x[0:N*navg],(N,navg)),axis=1)
```

# An example

So I (and you) can understand better how it works.

```python
x  = np.linspace(0,105,106)
x
```

```python
navg = 10
N = len(x) // navg
np.reshape(x[0:N*navg],(N,navg))
```

```python
np.average(np.reshape(x[0:N*navg],(N,navg)),axis=1)
```

# Moving average filter

A moving average filter that is reasonably fast? 

This one is handy:

https://stackoverflow.com/questions/13728392/moving-average-or-running-mean/27681394#27681394

```python
def moving_average(x, N):
    cumsum = np.cumsum(np.insert(x, 0, 0)) 
    return (cumsum[N:] - cumsum[:-N]) / float(N)
```

```python
x2 = np.array(range(10))
x2
```

```python
moving_average(x2,2)
```
