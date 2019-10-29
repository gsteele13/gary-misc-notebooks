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
```

# Thermal Conductivity of  Supports


## Specifications for RDK-305

http://www.shicryogenics.com/products/4k-cryocoolers/rdk-305d-4k-cryocooler-series/

1st stage capacity: 15W at 40K 

2nd stage capacity: 0.4W at 4.2K


## Estimating thermal heat load 

http://www.submm.caltech.edu/cso/receivers/thermal/thermal_integral_stainless.pdf

Thermal integral from 50K to 300K = about 30 W / cm

From 50K to 0K: 1.5 W / cm

Our legs: 20 cm long, 1 cm in diameter? 

```python
d = 1 # cm
l = 20 # cm
A = np.pi * (d/2)**2

print("Thermal leakage from RT to 50K stage: %.2f W" % (30*A/l))
print("Thermal leakage from RT to 50K stage: %.2f W" % (1.5*A/l))
```

# Radiative heat load

Heat flux $j$ in W/m^2:

$$
j = \sigma T^4
$$

Diameter of 4K plate: 30 cm

```python
d2 = 30e-2
sigma = 5.6e-8 # W/m^2/K^4
A2 = np.pi * (d2/2)**2 # Here: only top side
eps = 0.05 # estimated emissivitity of polished copper

print("Radiative heat load on 4K plate no shields:   %.3f W" % (eps*sigma*300**4*A2))
print("Radiative heat load on 4K plate with shields: %.3f W" % (eps*sigma*50**4*A2))
```

# Radiative heat load Entropy 300 mK

Heat flux $j$ in W/m^2:

$$
j = \sigma T^4
$$

* Diameter of old 300 mK plate: 5 cm ?
* Diameter of new 300 mK plate: 15 cm ?

Of course, the number for the old 300 mK plate is a massive underestimate since the radiation geometry is not dominantly "parallel plate". A "sphere in a sphere" would probably be a better estimate, but let's just check order of magnitude for now. 


```python
d1 = 5e-2
d2 = 15e-2
sigma = 5.6e-8 # W/m^2/K^4
A1 = np.pi * (d1/2)**2 
A2 = np.pi * (d2/2)**2 
eps = 0.05 # estimated emissivitity of polished copper

print("Radiative heat load 5 cm : %.3f uW" % (A1*eps*sigma*4**4*1e6))
print("Radiative heat load 15 cm: %.3f uW" % (A2*eps*sigma*4**4*1e6))
```
