---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.15.2
  kernelspec:
    display_name: Python (gary_default)
    language: python
    name: gary_default
---

Starting point:

https://nbviewer.org/urls/qutip.org/qutip-tutorials/tutorials-v4/time-evolution/006_photon_birth_death.ipynb

```python
import matplotlib.pyplot as plt
import numpy as np
from qutip import *

# fock space size
N = 5
# Destroy operator
a = destroy(N)
# oscillator Hamiltonian
H = a.dag() * a
# Initial Fock state with one photon
psi0 = basis(N, 1)

kappa = 1.0 / 0.129  # Coupling rate to heat bath
nth = 0.0  # Temperature with <n>=0.063

# collapse operators for the thermal bath
c_ops = []
c_ops.append(np.sqrt(kappa * (1 + nth)) * a)
c_ops.append(np.sqrt(kappa * nth) * a.dag())

ntraj = [1, 5, 15, 904]  # number of MC trajectories
tlist = np.linspace(0, 0.8, 100)

# Solve using MCSolve for different ntraj
mc = mcsolve(H, psi0, tlist, c_ops, [a.dag() * a], ntraj)
me = mesolve(H, psi0, tlist, c_ops, [a.dag() * a])
```

```python
plt.plot(tlist, mc.expect[0][0],'grey', label="single trajectory")
plt.plot(tlist, me.expect[0], "r--", lw=2)
plt.legend()
```

```python
import matplotlib.pyplot as plt
import numpy as np
from qutip import *

# fock space size
N = 20
# Destroy operator
a = destroy(N)
# oscillator Hamiltonian
H = a.dag() * a
# Initial Fock state with 5 photons
psi0 = coherent(N, 3)

kappa = 1.0 / 0.129  # Coupling rate to heat bath
nth = 0.0  # Temperature with <n>=0.063

# collapse operators for the thermal bath
c_ops = []
c_ops.append(np.sqrt(kappa * (1 + nth)) * a)
c_ops.append(np.sqrt(kappa * nth) * a.dag())

ntraj = [1, 5, 15, 904]  # number of MC trajectories
tlist = np.linspace(0, 0.8, 100)

# Solve using MCSolve for different ntraj
mc = mcsolve(H, psi0, tlist, c_ops, [a.dag() * a], ntraj)
me = mesolve(H, psi0, tlist, c_ops, [a.dag() * a])
```

```python
plt.plot(tlist, mc.expect[0][0],'grey', label="single trajectory")
plt.plot(tlist, me.expect[0], "r--", lw=2)
plt.legend()
```

OK, this gives nonsense. What if I also add in a photon number collapse operator? 

```python
import matplotlib.pyplot as plt
import numpy as np
from qutip import *

# fock space size
N = 20
# Destroy operator
a = destroy(N)
# oscillator Hamiltonian
H = a.dag() * a
# Initial Fock state with 5 photons
psi0 = coherent(N, 3)

kappa_decay = 1.0 / 0.129  
kappa_meas = kappa_decay*10

# decay and measurement
c_ops = []
c_ops.append(np.sqrt(kappa_decay) * a)
c_ops.append(np.sqrt(kappa_meas) * a*a.dag())


ntraj = [1, 5, 15, 904]  # number of MC trajectories
tlist = np.linspace(0, 0.8, 1000)

# Solve using MCSolve for different ntraj
mc = mcsolve(H, psi0, tlist, c_ops, [a.dag() * a], ntraj)
me = mesolve(H, psi0, tlist, c_ops, [a.dag() * a])
```

```python
plt.plot(tlist, mc.expect[0][0],'grey', label="single trajectory")
plt.plot(tlist, me.expect[0], "r--", lw=2)
plt.legend()
```

Looks reasonable, and like the experiment. However, no quantum zeno, which I would perhaps naively expect since I am collapsing 10 times faster than I am decaying. 


# Quantum zeno with qubit measurement

This is the one that confused me a while ago. I would expect to be able to zeno things using sigma_z: 

https://docs.qojulia.org/examples/quantum-zeno-effect/#Quantum-Zeno-Effect

Let's try a TLS and then compare Rabi oscillations and decay. And see if we get a trajectory that makes sense. 

```python
import matplotlib.pyplot as plt
import numpy as np
from qutip import *

eps = 1
H = eps*sigmax()
psi0 = basis(2,1)

kappa_meas = 1
c_ops = []
c_ops.append(np.sqrt(kappa_meas) * sigmaz())

ntraj = [1, 5, 15, 904]  # number of MC trajectories
tlist = np.linspace(0, 5, 1000)

p1 = projection(2,1,1)

me1 = mesolve(H, psi0, tlist, [], p1)
me2 = mesolve(H, psi0, tlist, c_ops, p1)
mc = mcsolve(H, psi0, tlist, c_ops)
```

```python
plt.plot(me1.expect[0])
plt.plot(me2.expect[0])
plt.plot(expect(p1,mc.states[0]), 'gray')
```

Nope!!!
