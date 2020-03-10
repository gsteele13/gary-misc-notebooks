---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.3.2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import numpy as np
import matplotlib.pyplot as plt
```

# Some simple examples of making multipanel plots for publications with gridspec()

See also: 

https://matplotlib.org/tutorials/intermediate/gridspec.html

For our publications, we want to have careful control over the size of our panels, since we are constrained by the journal. For example, typical half column widths are 86 mm = 3 3/8 inches = 3.375 inches.

https://journals.aps.org/prl/info/infoL.html

Here, I will look at some examples where I use gridspect to build a panel layout of a figure with specific sizes.


## Example 1

```python
fig = plt.figure(figsize=(3,4)) # matplotlib uses inches
widths = [1.5,1.5]
heights = [1,2,2]
spec = fig.add_gridspec(ncols=2, nrows=3, width_ratios=widths, height_ratios=heights)
```

```python

```
