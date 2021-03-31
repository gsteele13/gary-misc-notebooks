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
import json

class Foo:
    a = 1
    b = 1.5
    c = "a string"
    d = ["a", "list", "of", "strings"]
    
    def save_settings(self):
        settings = {}
        attributes = dir(self)
        for k in attributes: 
            if k[0] != "_" and not callable(eval("self."+k)):
                settings[k] = eval("self."+k)
        with open("settings.json", "w") as f:
            json.dump(settings, f, indent=4)
    
    def load_settings(self):
        with open("settings.json", "r") as f:
            settings = jason.load(f)
        # Backward compatibility could be handled here by 
        # using an except() statement, as long as we don't
        # rename things (in which case we would need to 
        # maintain a name mapping dict or something like 
        # that)
        for k in settings.keys():
            exec(f"self.{k} = settings['{k}']")       
```

```python
F = Foo()
F.a = 100
```

```python
F.save_settings()
```

```python
F.a = 53
```

```python
F.a
```

```python
F.load_settings()
```

```python
F.a
```
