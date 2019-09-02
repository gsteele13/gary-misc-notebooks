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
from scipy.linalg import toeplitz
import numpy as np
import matplotlib.pyplot as plt
```

```python
def print_matrix(A, s=0, e=-1):
    B = A[s:e,s:e]
    for i in range(B.shape[0]):
        for j in range(B.shape[1]):
            #print("%  11.2e" % B[i,j], end='')
            print("%  7.1f" % B[i,j], end='')
        print()
```

```python
N = 100

vec1 = np.zeros(N)
vec2 = np.zeros(N)
vec1[1] = 1
vec2[1] = -1
D = toeplitz(c=vec2, r=vec1)
T = -np.matmul(D,D)

#print("Matrix T:")
#print_matrix(T, e=N)
#print()

val, vec = np.linalg.eig(T)

#en = val;
#print("Eigenvalues:")
#print(en)
#print()
#print("Why are there two energies for each? ISW should not be 2-fold degenerate?")

plt.plot(vec[:,53], 'o-')
plt.show()

print("I would expect these shouls look a bit like sine waves. Why does this look like \n"
     "total nonsense?")
```

```python
val, vec = np.linalg.eig(T)
test = np.matmul(T, vec[1])
print(np.array([test, vec[1]]).T)
```

```python
M = np.array([[ 1,  0, -1,  0,  0 ], 
              [ 0,  2,  0, -1,  0 ], 
              [-1,  0,  2,  0, -1], 
              [ 0, -1,  0,  2,  0], 
              [ 0,  0, -1,  0,  1]])
print_matrix(M, e=5)
val, vec = np.linalg.eigh(M)
vec[:,0]
test = np.matmul(M, vec[:,0])
print("\nleft eigv, rigth M matmul eigv \n")
print(np.array([ vec[:,0], M.dot(vec[:,0])]).T)
print("\nSeems an awful lot like it is not an eigenvector?")
```

```python

```

```python
T.shape
```

Update: Anton helped me figure it out!

Upshot: you should not use center difference definition for derivative since it splits your grid into two decoupled sublattices. 

Trick from Jos: `np.eye` is a great tool for this!
