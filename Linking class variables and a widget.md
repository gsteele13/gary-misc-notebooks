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
    P.update_plot()
a_slider.observe(a_slider_cb)
display(a_slider)
```
