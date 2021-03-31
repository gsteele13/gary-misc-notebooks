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

# Hand-coded callbacks implementation

```python
import matplotlib.pyplot as plt
import numpy as np
import ipywidgets as widgets

%matplotlib ipympl

class Plotter:
    a = 1
    b = 1
    x = np.linspace(0,10)
    
    def open_plot(self):
        fig, ax = plt.subplots()
        self.line, = ax.plot(x,self.a*np.sin(x+self.b))

    def update_plot(self):
        self.line.set_data(x,self.a*np.sin(x+self.b))

P = Plotter()
P.open_plot()

# This is a lot of ugly boiler plate. 
#
# Is there some way to "link" the slider widget to the class attribute? 
# And link an action to be taken (callback?) in the class when the 
# value of an attribute changes? 
#
# ie. in this case somehow in the class specify that update_plot() should
# be called if the value of a changes? 

a_slider = widgets.FloatSlider(min=0, max=1, step=0.01, value=P.a)
def a_slider_cb(w):
    P.a = a_slider.value
a_slider.observe(a_slider_cb)
display(a_slider)
```

# Example using trailets

From Anton's suggestion:

https://forum.kavli.tudelft.nl/t/linking-widgets-to-class-attributes-and-class-attributes-to-actions/160/2


```python
import matplotlib.pyplot as plt
import numpy as np
import ipywidgets as widgets
from traitlets import HasTraits, link, Float, observe

%matplotlib ipympl

# Plotter is in this way a subclass of HasTraits
class Plotter(HasTraits):
    a = Float(1.0)
    b = Float(0)
    x = np.linspace(0,10)
    
    def open_plot(self):
        fig, ax = plt.subplots()
        self.line, = ax.plot(x,self.a*np.sin(x+self.b))

    @observe('a', 'b')
    def update_plot(self, value):
        self.line.set_data(x,self.a*np.sin(x+self.b))

# Create object, open plot
P = Plotter()
P.open_plot()

# Trailet linking, this is a lot nicer!
a_slider = widgets.FloatSlider(min=0, max=1, step=0.01, value=P.a)
link((a_slider, "value"), (P, "a"))
b_slider = widgets.FloatSlider(min=0, max=np.pi, step=0.01, value=P.b)
link((b_slider, "value"), (P, "b"))

# Display widgets
display(widgets.HBox([a_slider, b_slider]))
```

```python
P.a = 0
```

```python

```
