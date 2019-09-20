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

# Immutable

```python
# Example: Local scope of a variable created in a function

terminal_velocity = 5.3 

def set_velocity():
    terminal_velocity = 20
    print("Inside function: ", terminal_velocity)
     
set_velocity()
print("Outside function:", terminal_velocity)
```

```python
# Example: Go up to Global scope when looking up a variable that 
# does not exist in the local scope

terminal_velocity = 5.3 

def set_velocity():
    print("Inside function: ", terminal_velocity)

set_velocity()
print("Outside function:", terminal_velocity)
```

```python
# Example: In this case, from the assignment statement, the scope of 
# the variable becomes local. But then we get an error because
# python thinks we are trying to use the value of a variable before 
#  it is created

terminal_velocity = 5.3 

def set_velocity():
    print("Inside function 1:", terminal_velocity)
    terminal_velocity = 20
    print("Inside function 2:", terminal_velocity)

     
set_velocity()
print("Outside function:", terminal_velocity)

```

```python
# Example: In this case, from the assignment statement, the scope of 
# the variable becomes local. But then we get an error because
# python thinks we are trying to use the value of a variable before 
#  it is created

terminal_velocity = 5.3 

def set_velocity():
    print("Inside function 1:", terminal_velocity)
    terminal_velocity = 20
    print("Inside function 2:", terminal_velocity)

     
set_velocity()
print("Outside function:", terminal_velocity)
```

# Mutable vs immutable

```python
# Immutable varibles work always with call-by-value: the variable inside the function is a new variable
# that is created and the value of the passed variable is copied into it

terminal_velocity = 5.3

def set_velocity(terminal_velocity):
    terminal_velocity = 20
     
set_velocity(terminal_velocity)

print(terminal_velocity)
```

```python
# Mutable variables work with call-by-reference: the varible inside the function has a new name
# but points to the original VALUE in memory where the passed variable is stored. Changing the 
# value of one of the elements in memory changes the value of  the variable in the global scope!

terminal_velocity = [5.3] 

def set_velocity(x):
    x[0] = 20
     
set_velocity(terminal_velocity)

print(terminal_velocity)
```

```python
# And now another subtlety: if you assign the local variable to a NEW object, as is done here, then 
# it leave the original memory that the global variable points to untouched

terminal_velocity = [5.3] 

def set_velocity(x):
    x = [20]
     
set_velocity(terminal_velocity)

print(terminal_velocity)
```
