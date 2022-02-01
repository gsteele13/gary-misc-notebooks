---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.6
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python
import glob
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import string
```

# The final function

```python
def load_vcl(file):
    with open(file, "rb") as f:
        byte_stream = bytearray(f.read())

    column_names_bytes = byte_stream[0x400+0x1400:0x400+0x1400+0x1700]
    column_names = list()

    for i in range(184):
        b = column_names_bytes[i*32:(i+1)*32]
        if b[0] != 0:
            s = b.decode().rstrip("\x00")
            column_names.append(s)

    # This is where data starts
    data_bytes = byte_stream[0x3000:]
    # Truncate it to a full row
    bytes_full_records = int(len(data_bytes)/8/len(column_names))*8*len(column_names)
    data_bytes = data_bytes[0:bytes_full_records]
    # Now convert from bytes to double floating point and reshape
    data = np.frombuffer(data_bytes, 'd')
    data = np.reshape(data, [len(data)//len(column_names),len(column_names)])
    return pd.DataFrame(data, columns=column_names)
```

# The debugging process

```python
base_path = "/home/jovyan/cryo_logs/triton_logs/LogFiles/58487 Steele/"
vcl_files = glob.glob(base_path + "*.vcl")
```

```python
file = vcl_files[-1]
file
```

```python
with open(file, "rb") as f:
    byte_stream = bytearray(f.read())
```

```python
type(byte_stream)
```

```python
len(byte_stream[0:0x3000])
```

```python
header = byte_stream[0:0x400]
print(header.decode())
```

```python
comments = byte_stream[0x0400:0x0400+0x1400]
print(comments.decode())
```

```python
column_names_bytes = byte_stream[0x400+0x1400:0x400+0x1400+0x1700]
column_names = list()

for i in range(184):
    #print(column_names[i*32:(i+1)*32])
    b = column_names_bytes[i*32:(i+1)*32]
    if b[0] != 0:
        s = b.decode().rstrip("\x00")
        print(i, s)
        column_names.append(s)
    #print("%d %d _%s_" % (len(s), s))
```

```python
column_names[0]
```

```python
data_bytes = byte_stream[0x3000:]
```

How many columns? 

> The LineSize(bytes) value (converted to an integer count) should be equal the count of non-empty Column Names times 8.

I should be able to figure this out from the number of columns with non-empty column names, and also cross check with the (identical) entry of LineSize in each of the rows. 

```python
len(column_names)*8
```

```python
import struct
```

```python
struct.unpack('d', data_bytes[0:8])
```

OK, this works!

Now, just to do this vectorised! :)

```python
# Slice out data, truncate at last full record
data_bytes = np.frombuffer(byte_stream[0x3000:], dtype='d')
bytes_full_records = int(len(data)/8/len(column_names))*8*len(column_names)
data_bytes = data[0:bytes_full_records]

# Reshape into appropriate array
data = np.frombuffer(data_bytes, 'd')
data = np.reshape(data, [len(data)//len(column_names),len(column_names)])

# Make a dataframe
df = pd.DataFrame(data, columns=column_names)
```

```python
df.keys()
```

```python
hours = (df['Time(secs)']-df["Time(secs)"][0])/3600
plt.plot(hours, df['MC Plate T(K)'])
plt.yscale('log')
```

OK, we're good to go!
