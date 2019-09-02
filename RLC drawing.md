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
import SchemDraw as schem
import SchemDraw.elements as e
```

```python
d = schem.Drawing()
d.add(e.GND)
V1 = d.add(e.INDUCTOR, label='$L$')
R = d.add(e.RES, label='$R$')
cs = d.add(e.CAP, d='down', botlabel='$C$')
d.add(e.GND)
d.draw()
```

```python

```
