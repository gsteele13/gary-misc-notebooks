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
%matplotlib notebook
```

```python
%matplotlib inline
plt.rcParams['figure.dpi'] = 100
```

```python
x = np.linspace(-500,500,5000)

def peak(x0, w):
    return np.exp(-((x-x0)/w)**2/2)

w0 = 2
orig = peak(-10,w0) + peak(10,w0)

w_psf = 100
psf = peak(0,w_psf)/np.sqrt(2*np.pi)/w_psf

plt.plot(x,orig)
plt.plot(x,psf)
```

```python

def convolve(y1,y2):
    n = 2*len(y1)
    y1t = np.fft.fft(y1, n=n)
    y2t = np.fft.fft(y2, n=n)
    return np.fft.ifft(y1t*y2t, n=n)[0:len(y1)]

smeared = convolve(orig, psf)

plt.plot(x,smeared)
```
