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

```python
from qutip import *
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
plt.rcParams["animation.html"] = "jshtml"
```

```python
def make_animation(psi):
    fig,ax = plt.subplots()
    fig.clear()
    b = Bloch(fig=fig)

    def update(n):
        b.clear()
        b.add_states(psi[n])
        b.make_sphere()

    anim = animation.FuncAnimation(fig, update, frames=len(psi), interval=10)
    plt.close(fig)
    return(anim)
```

# Qubit on it's own (boring, does nothing...)

```python
H = 0.0 * sigmaz() 
psi0 = (basis(2,0) + basis(2,1)).unit()
t = np.linspace(0,10,100)
result = sesolve(H, psi0, t)
psi = result.states
```

```python
rho12 = expect(projection(2,0,1), psi)
plt.plot(np.real(rho12))
plt.plot(np.imag(rho12))
```

```python
make_animation(psi)
```

# A qubit coupled to a 1-qubit environment


Start with environment in a definite state:

```python
sigmaz_q = tensor(sigmaz(), identity(2)) # the qubit
sigmaz_1 = tensor(identity(2), sigmaz()) # the first environment spin
H = 1.0 * sigmaz_q * sigmaz_1

psi0_qubit = (basis(2,0) + basis(2,1)).unit()
psi0_env = basis(2,0)

psi0 = tensor(psi0_qubit, psi0_env)

t = np.linspace(0,10,100)
result = sesolve(H, psi0, t)
psi = result.states

rho_qubit = [ptrace(k,0) for k in psi]
```

```python
make_animation(rho_qubit)
```

```python
rho12 = expect(projection(2,0,1), rho_qubit)
plt.plot(np.real(rho12))
plt.plot(np.imag(rho12))
```

This one is just boring: it is just a shift in the qubit frequency. 


Now try environment in a superposition. This should be interesting as it it will induce a "quantum uncertainty" in the qubit frequency, in some sense

```python
sigmaz_q = tensor(sigmaz(), identity(2)) # the qubit
sigmaz_1 = tensor(identity(2), sigmaz()) # the first environment spin
H = 1.0 * sigmaz_q * sigmaz_1

psi0_qubit = (basis(2,0) + basis(2,1)).unit()
psi0_env = (basis(2,0) + basis(2,1)).unit()

psi0 = tensor(psi0_qubit, psi0_env)

t = np.linspace(0,10,100)
result = sesolve(H, psi0, t)
psi = result.states

rho_qubit = [ptrace(k,0) for k in psi]

rho12 = expect(projection(2,0,1), rho_qubit)
plt.plot(np.real(rho12))
plt.plot(np.imag(rho12))
```

```python
make_animation(rho_qubit)
```

Of course, it has no effect on the basis states: this is why pointer states do not decohere!

```python
sigmaz_q = tensor(sigmaz(), identity(2)) # the qubit
sigmaz_1 = tensor(identity(2), sigmaz()) # the first environment spin
H = 1.0 * sigmaz_q * sigmaz_1

psi0_qubit = (basis(2,0)).unit()
psi0_env = (basis(2,0) + basis(2,1)).unit()

psi0 = tensor(psi0_qubit, psi0_env)

t = np.linspace(0,10,100)
result = sesolve(H, psi0, t)
psi = result.states

rho_qubit = [ptrace(k,0) for k in psi]
make_animation(rho_qubit)
```

# A qubit coupled to a two-qubit environment

```python
sigmaz_q = tensor(sigmaz(), identity(2), identity(2)) # the qubit
sigmaz_1 = tensor(identity(2), sigmaz(), identity(2)) # the first environment spin
sigmaz_2 = tensor(identity(2), identity(2), sigmaz()) 

# With different coupling rates of course
H = 1.0 * sigmaz_q * sigmaz_1 + 0.6 * sigmaz_q * sigmaz_2

psi0_qubit = (basis(2,0) + basis(2,1)).unit()
psi0_env1 = (basis(2,0) + basis(2,1)).unit()
psi0_env2 = (basis(2,0) + basis(2,1)).unit()

psi0 = tensor(psi0_qubit, psi0_env1, psi0_env2)

t = np.linspace(0,10,100)
result = sesolve(H, psi0, t)
psi = result.states

rho_qubit = [ptrace(k,0) for k in psi]
```

```python
make_animation(rho_qubit)
```

```python
plt.plot(t, expect(sigmax(), rho_qubit))
plt.axhline(0,ls=":",c='grey')
```

Adding more of these will result in the exponential supression of coherence discussed by Zurek in the review article: 

Decoherence, einselection, and the quantum origins of the classical <br>
Wojciech Hubert Zurek <br>
Rev. Mod. Phys. 75, 715 – Published 22 May 2003 <br>
https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.75.715

It also also the same origin of other apparent non-unitary evolution in a coherent thermodynamic limit. I wrote a term paper about this years ago at MIT (you can find it [here](https://zenodo.org/records/5732307) if you are interested), and was recently explore numerically by a group of students in the course TN3156 "Quantum Sensing and Measurement": 

https://nsweb.tn.tudelft.nl/~gsteele/misc/QSM_Project_Unitary_Exponential%20Decay.html

If we scale up the size of the environment, with a random distribution of couplings, and a random distribution of initial states, we should be able to see a convergence to an exponential suppression of coherence of superposition states of the environment. 

To do for future additions to this notebook!

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

# Analytical formula from Zurek


![IMG_1590.jpg](attachment:IMG_1590.jpg)


![IMG_1591.jpg](attachment:IMG_1591.jpg)

```python
N_values = np.append(np.array([0,1,]*10), np.array(range(100)))

fig,ax = plt.subplots(figsize=(12,4))
fig.clear()

t = np.linspace(0,500,5000)
g_std = 0.1 # how much variation in g do we allow
r = np.ones(len(t))*1j

N = 0
plt.subplot(121)
l1, = plt.plot(t,np.real(r), ":")
l2, = plt.plot(t,np.imag(r), ":")
l3, = plt.plot(t,np.abs(r))
plt.ylim(-1.1,1.1)
plt.ylabel("Real / Imag / Abs r(t)")
plt.xlabel("t")
title = plt.title("N = %d" % N)
plt.subplot(122)
l4, = plt.plot(t,np.real(r), ":")
l5, = plt.plot(t,np.imag(r), ":")
l6, = plt.plot(t,np.abs(r))
plt.ylabel("Real / Imag / Abs r(t)")
plt.xlabel("t")
plt.xlim(0,10)
plt.ylim(-1.1,1.1)
    
def update(i):
    r = np.ones(len(t))*1j
    N = N_values[i]
    for i in range(N):
        g_k = np.random.normal()*g_std
        beta_k_squared = np.random.random()
        r *= np.cos(2*g_k*t) + 1j*(1-2*beta_k_squared)*np.sin(2*g_k*t)
    l1.set_data(t,np.real(r))
    l2.set_data(t,np.imag(r))
    l3.set_data(t,np.abs(r))
    l4.set_data(t,np.real(r))
    l5.set_data(t,np.imag(r))
    l6.set_data(t,np.abs(r))
    title.set_text("N = %d" % N)

anim = animation.FuncAnimation(fig, update, frames=len(N_values), interval=500)
plt.close(fig)
```

I have made an animation of an interesting sequence of changing N here below. I start by switching between 0 and 1, to illustrate the random nature with a single environment degree of freedom, then scroll through increasing N up to 100. There are two plots, one showing times up to "500" (in some unit related to g) and one zoomed in in the range of times from 0 to 10, where you can see the typical timescale on which coherence is destroyed even for moderate N. 

```python
anim
```

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

# Appendix: Re-figuring-out how Bloch sphere animations work

```python
from qutip import *
import numpy as np
import matplotlib.pyplot as plt


fig,ax = plt.subplots()
fig.clear()
b = Bloch(fig=fig)

def update(n):
    b.clear()
    psi = (basis(2,0) + np.exp(1j*n/100)*basis(2,1)).unit()
    b.add_states(psi)
    b.make_sphere()

anim = animation.FuncAnimation(fig, update, frames=100, interval=10)
plt.close(fig)
anim
```

```python
# Save to gif
anim.save('block_sphere_demo.gif',fps=20)
from IPython.display import Image
Image("block_sphere_demo.gif")
```
