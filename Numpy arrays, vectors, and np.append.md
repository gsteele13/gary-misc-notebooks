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
```

# A small note about numpy arrays vs matrices, and their behaviour with `np.append()`

## Column vs. Row vectors

First, let's start with vectors (arrays). 

In 2D (ie. two index) matrix mathematics, you can have two types of vectors: row vectors and column vectors, and you can change a vector from one type to the other using the transpose. 

There is even a long stack exchange discussion about this:

https://stackoverflow.com/questions/17428621/python-differentiating-between-row-and-column-vectors

In numpy, this can also be true, but you need to use 2D numpy arrays: as the post points out, a transpose in 1D matrices does not make sense, and "column" and "row" vectors only make sense if they are "2d". Let's have a bit of a look at this. 

### 1D numpy arrays


If you make an array from a "flat list", then it is becomes a 1D array:

```python
a = np.array([0,1])
a.shape
```

We can see the the dimensionality here:

```python
len(a.shape)
```

And note that the transpose is equal to itself:

```python
print(a)
```

```python
print(a.T)
```

If I append 1D arrays together, you get a concatenated 1D array:

```python
print(np.append(a,a))
```

### 2D numpy arrays


To make a 2D numpy array from a list, you need to create a "list of lists". For example a 2x2 array is then:

```python
b = np.array([[1,1],[0,1]])
b.shape
```

```python
len(b.shape)
```

```python
print(b)
```

```python
print(b.T)
```

If we want to create column and row vectors, we can do that by making a list of lists that contains only one list:

```python
c = np.array([[0,1]])
print(c)
```

This looks a lot like our original array `a` above, but it's actually a different shape:

```python
c.shape
```

And now if we take the transpose of `c` we get something different!

```python
print(c.T)
```

A column vector! And it has a different shape, (2,1) instead of (1,2):

```python
print(c.T.shape)
```

Appending them unfotunately does not do what you might think it should: it flattens everything out into a 1D array:

```python
d = np.append(c,c)
print(c)
print(d)
print(d.shape)
```

This is actually mentioned in the documentation:

https://numpy.org/doc/stable/reference/generated/numpy.append.html

You can change this by using the `axis` parameter. 

It seems that (contrary to the documentation...?) that the default is actually `axis=None`:

```python
d = np.append(c,c, axis=None)
print(c)
print(d)
print(d.shape)
```

If we change it to `axis=0`, we can stack our row vector `c` in the vertical direction on top of each other:

```python
d = np.append(c,c, axis=0)
print(c)
print(d)
print(d.shape)
```

And if we choose `axis=1` we can stack them beside each other:

```python
d = np.append(c,c, axis=1)
print(c)
print(d)
print(d.shape)
```

### Another hack to get a 2D array: append them into a list then convert to array

This works with either 1D lists:

```python
e = [0,1] # a list
f = np.array([e,e,e,e,e]) # a list of lists becomes a 2D numpy array
print(f.shape)
print(f)
```

and a list of 1D numpy arrays:

```python
g = np.array([0,1])
h = np.array([g,g,g,g,g])
print(h.shape)
```

And this works then fine using the list `append()` function in a loop:

```python
l = []
for i in range(5):
    l.append(e)
m = np.array(l)
print(m.shape)
print(m)
```
