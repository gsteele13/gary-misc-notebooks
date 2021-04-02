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

# My first hack

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
F.a
```

```python
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

# An improvement from Slava

```python
import json

class Foo:
    def __init__(self):    
        self.a = 1
        self.b = 1.5
        self.c = "a string"
        self.d = ["a", "list", "of", "strings"]

    def save_settings(self):
        print(self.__dict__)
        with open("settings.json", "w") as f:
            json.dump(self.__dict__, f, indent=4)
    
    def load_settings(self):
        print(self.__dict__)
        with open("settings.json", "r") as f:
            settings = json.load(f)
        self.__dict__.update(settings)
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
F.a
```

```python
F.load_settings()
F.a
```

```python
F.__dict__
```

# Reverse compatibility? 

```python
import json

class Foo2:
    def __init__(self):    
        self.a = 1
        self.b = 1.5
        self.c = "a string"
        self.d = ["a", "list", "of", "strings"]
        self.e = "new"

    def save_settings(self):
        print(self.__dict__)
        with open("settings.json", "w") as f:
            json.dump(self.__dict__, f, indent=4)
    
    def load_settings(self):
        print(self.__dict__)
        with open("settings.json", "r") as f:
            settings = json.load(f)
        self.__dict__.update(settings)
```

```python
F2 = Foo2()
```

```python
F2.load_settings()
```

# Python class strangeness...

```python
class Bar:
    a = 1
    b = 10
```

```python
B = Bar()
```

```python
B.__dict__
```

```python
B.a
```

```python
B.__dict__
```

```python
B.a = 10
```

```python
B.__dict__
```
