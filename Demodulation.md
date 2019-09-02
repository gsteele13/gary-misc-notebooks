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

Using the FFT, we can find the steady state frequency that our numerical integration calculation produces, and find that is close to what we would predict (specifically, that the frequency of oscillation is the same as the driving frequency). 

How can we check the phase of the oscillations? In principle, this is also contained in the phase of the y complex-valued Fourier transform. However, due to the finite sampling of the frequencies in Fourier space from the limited trace lenght, these can be tricky to extract accurately unless the sampling frequency is an exact multiple of the frequency we are interested in. 

Fortunately, if we know the frequency that we are interested in, we can play a little trick (actually a form of <a href=https://en.wikipedia.org/wiki/Demodulation>demodulation</a>). Let's say I have a signal:

$$
s(t) = A \cos(\omega t + \phi)
$$ 

Using double angle formulas, I can re-write this as:

\begin{eqnarray}
s(t) & = &  A \cos(\phi) \cos(\omega t) - A \sin(\phi) \sin(\omega t)
 & \equiv & I \cos(\omega t) + Q \cos(\omega t)
\end{eqnarray}

where $I$ and $Q$ are called the in and out of phase <a href=https://en.wikipedia.org/wiki/In-phase_and_quadrature_components>quadrature</a> components of the signal. 

I know the frequency $\omega$ of the signal, but I want to find the phase $\phi$ (and maybe also amplitude $A$). To this we can use the following trick: let's multiply $s(t)$ by $\cos(\omega t)$. If I do this, I will get the following:

$$
s(t) \cos(\omega t) = I \cos(\omega t) \cos(\omega t) + Q \sin(\omega t) \cos(\omega t)
$$

Now the trick: if I integrate this over an integer number of periods of oscillation, then the second term will cancel! However, the first term will not: you can see this using the <a href=https://en.wikipedia.org/wiki/List_of_trigonometric_identities#Double-angle_formulae>double angle formulas</a>: 

$$
\cos^2(A) = (1+\cos(2A))/2
$$

Because of the 1, this will survive the integration! We can the calculate the $I$ quadrature of the signal using:

$$
I = \frac{2}{NT} \int_t^{t+NT} s(t) \cos(\omega t) dt
$$

where $T = 2\pi / \omega$ is the period of oscillation of the frequency you are trying to detect and $N$ is an integer. One can similarly show that:

$$
Q = \frac{2}{NT} \int_t^{t+NT} s(t) \sin(\omega t) dt
$$

Once I have $I$ and $Q$, I can calculate the amplitude and phase of my signal using:

$$
A = \sqrt{I^2 + Q^2} \\
\phi = \arctan(Q/I)
$$

**Exercise 1(c)** Calculate the phase of the oscillations using the Fourier transform.

