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
import matplotlib.pyplot as plt
```

# Duffing Oscillator Bifurcation

In this notebook, we will look a bit at the bifurcation of the Duffing oscillator. We will do this using the "slowly varying envelope approximation", equivalent to the RWA. We will follow this reference:

The bifurcations of duffing's equation: An application of catastrophe theory <br>
P.J.Holmes, D.A.Rand <br>
Journal of Sound and Vibration <br>
Volume 44, Issue 2, 22 January 1976, Pages 237-253 <br>
Journal of Sound and Vibration <br>
https://doi.org/10.1016/0022-460X(76)90771-9

We will parametrise the equation as:

$$
\ddot x + 2 \zeta \dot x + x + \alpha x^3 = \phi \cos \Omega t 
$$

In this form, the natural frequency $\omega_0 = 1$. 

We will consider motion of the oscillator: 

$$
x(t) = A(\Omega) \cos(\Omega t + \theta)
$$

We can derive the solutions for the amplitude by finding the roots of a characteristic polynomial. Defining a variable $y(\Omega) = (A(\omega))^2$ representing the square amplitude, Holmes shows that the value of $y$ is determined by the roots of the following polynomial:

$$
 9 \alpha^2 y^3 + 24 \alpha \rho y^2 + 16(\rho^2+\sigma^2)y - 16 \phi^2 = 0
$$

where  $\rho = 1 - \Omega^2$ and $\sigma = 2 \zeta \Omega$. We can then also calculate the phase $\theta$ using:

$$
\theta =  \arcsin(-\sigma A /\phi)
$$

In this notebook, we will find, plot, and explore the solutions of this as a function of driving frequency and force. 

For this, we will use the `roots()` function from numpy:

https://numpy.org/doc/stable/reference/generated/numpy.roots

```python
def find_amp_phase(Omega, phi, Q = 100, alpha = 1):
    zeta = 0.5/Q
    rho = 1-Omega**2
    sigma = 2*zeta*Omega
    p = np.zeros(4)
    p[0] = 9*alpha**2
    p[1] = 24*alpha*rho
    p[2] = 16*(rho**2+sigma**2)
    p[3] = -16*phi**2
    y_roots = np.roots(p)
    A = np.sqrt(y_roots)
    theta = np.arcsin(-sigma*A/phi)
    return A,theta
```

Quick test: 

```python
Omega = 1
phi = 1
find_amp_phase(Omega,phi,100,0)
```

## Check with linear HO ($\alpha = 0$)

```python
phi = 1
Omega = np.linspace(0.9,1.1,1000)
Q = 100
alpha = 0

A = []
theta = []
for O in Omega:
    a,t = find_amp_phase(O,phi,Q,alpha)
    A.append(a)
    theta.append(t)
A = np.array(A)
theta = np.array(theta)
plt.plot(Omega,A)
plt.xlabel("$\Omega$")
plt.ylabel("$A$")
plt.show()
plt.plot(Omega,theta/np.pi)
plt.xlabel("$\Omega$")
plt.ylabel(r"$\theta$")
plt.show()
```

Hmmm...the second plot does not look quite right? Not sure, it seems to match the Holmes and Rand formula? But above the resonance frequency, the phase should asymptote to $-\pi$? 


## Check with alpha = 1e-3, phi = tiny

This should also reduce to the linear HO

```python
phi = 1e-9
Omega = np.linspace(0.5,2,1000)
alpha = 1e-99
Q = 10

A = []
theta = []
for O in Omega:
    a,t = find_amp_phase(O,phi,Q,alpha)
    A.append(a)
    theta.append(t)
A = np.array(A)
theta = np.array(theta)
plt.plot(Omega,np.real(A))
plt.xlabel("$\Omega$")
plt.ylabel("Real part of $A$")
plt.show()
plt.plot(Omega,np.imag(A))
plt.xlabel("$\Omega$")
plt.ylabel("Imag part of $A$")
plt.show()
```

OK, this is giving nonsense. 
