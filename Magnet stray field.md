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

http://hyperphysics.phy-astr.gsu.edu/hbase/magnetic/curloo.html

Let's do a rough estimate. If $B_0$ is the field at the middle of the ring, then the field on axis in z-direction at large distances is:

$$
B =  \frac{B_0 R^3}{Z^3}
$$

Let's take $B_0 = 10$ T. 

```python
B0 = 0.05
R = 5e-2 # 5 cm
Z = 50 # meters

print(B0*R**3/Z**3/1e-9, "nT")
```

```python

```
