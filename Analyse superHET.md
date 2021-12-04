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
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from bokeh.plotting import figure, show
from bokeh.io import output_notebook
from bokeh.models import CrosshairTool
output_notebook()

```

Copy paste from: 

https://www.minicircuits.com/pages/s-params/ZVBP-10R7G-S+_VIEW.pdf

```python
data = '''6000 103.14 110.59 102.45 0.06 0.14 0.16 0.09 0.18 0.22
6500 100.75 104.17 110.01 0.06 0.15 0.16 0.08 0.18 0.21
7000 104.80 104.13 103.43 0.04 0.15 0.16 0.08 0.18 0.21
7500 104.53 98.35 94.99 0.02 0.15 0.17 0.08 0.18 0.20
8000 96.74 94.60 99.38 0.00 0.14 0.18 0.08 0.19 0.22
8500 89.38 89.66 86.41 0.03 0.13 0.17 0.07 0.18 0.22
9300 67.73 67.60 67.41 0.07 0.11 0.18 0.09 0.23 0.29
10065 30.59 30.28 29.78 0.00 0.17 0.27 0.18 0.36 0.49
10180 20.51 20.03 19.39 0.12 0.26 0.37 0.26 0.48 0.63
10265 11.13 10.39 9.61 0.57 0.79 1.02 0.66 0.99 1.25
10330 3.64 3.01 2.46 3.12 4.12 5.24 3.13 4.22 5.42
10450 0.26 0.42 0.53 29.69 27.59 23.35 30.14 29.20 24.94
10550 0.22 0.39 0.49 28.20 26.36 26.33 30.62 27.42 27.07
10600 0.22 0.39 0.49 22.82 22.54 22.40 25.17 24.11 23.47
10700 0.20 0.36 0.47 27.57 34.03 32.92 30.55 40.96 40.36
10750 0.20 0.37 0.46 25.96 29.23 34.88 26.60 27.63 31.21
10800 0.21 0.38 0.47 23.98 25.24 28.78 25.02 25.36 28.39
10850 0.21 0.38 0.47 29.47 31.61 35.79 34.92 33.39 36.75
10900 0.23 0.39 0.49 25.74 29.79 29.62 26.99 30.43 29.84
10950 0.26 0.42 0.52 21.51 25.41 30.21 21.13 23.68 27.17
11080 2.38 3.17 4.16 4.61 3.85 3.02 4.66 3.96 3.19
11145 9.05 10.13 11.21 0.75 0.79 0.71 0.84 0.84 0.77
11240 19.07 20.11 20.95 0.05 0.22 0.25 0.18 0.30 0.34
11365 29.32 30.28 30.94 0.03 0.16 0.21 0.07 0.19 0.24
11800 52.07 52.74 53.26 0.10 0.11 0.20 0.09 0.25 0.31
12000 59.22'''
```

```python
d2 = [l.split(" ") for l in data.split('\n')]
```

```python
len(d2)
```

```python
df = pd.DataFrame(d2)
```

```python
df = df[0:-1]
```

```python
df
```

```python
f = df[0].values.astype(float)
db = df[1].values.astype(float)
```

```python
p = figure(height=300, width=600) 
p.sizing_mode = "scale_width"
p.line(f,-db)
target = show(p, notebook_handle=True)

```

```python
fi = np.arange(f[0],f[-1],1)
```

```python
dbi = np.interp(fi,f,db)
```

```python
f = 6300
dbi[np.where(fi == 6300)]
```

```python
f_if = 350
f_upper = 10500
f_car = f_upper - f_if
f_lower = f_upper - 2*f_if

for f in "f_upper", "f_upper - 25", "f_car", "f_lower":
    print("db %s %f" % (f, dbi[np.where(fi == eval(f))]))

```

That's probably not going to cut it in terms of carrier suppression. 
