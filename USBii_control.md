---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.3.0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import serial
import time
```

# Windfreak Tech SynthUSBii Generator

From:

https://windfreaktech.com/

Communication protocol details are included in the PDF on the USB stick that ships with the device:

Some of the important points:

>  Talking to the SynthUSBII unit is done through USB via a virtual serial / com port. The drivers supplied by WFT must be installed on your PC before communication can happen. After plugging in the hardware the com port will need to be identified, then used for any subsequent communication.
>
> The first character of any communication to the SynthUSBII unit is the command. (It is case sensitive.) What this character tells the unit to do is detailed below. Ideally a “package” is sent all at once. For example a communication for programming the frequency of the LO to 1GHz would be sent as “f1000.0” (without the quotes).
>
> For commands that return information from the SynthUSBII unit, such as reading the firmware version, it is advisable to send the command and then read the bytes returned fairly quickly to get them out of the USB buffer in your PC.
>
>Please keep in mind that the device expects the format shown. For example, if you send simply just an “f” the processor will sit there and wait for the rest of the data and may appear locked up. If you dont send the decimal point and at least one digit afterward, it will have unexpected results. Also, please send data without hidden termination characters such as a carriage return at the end.


```python
# OK, no termination characters, no handshaking, so we should just timeout apparently on reads...mega flaky?
# It means that any read command will ALWAYS take 300 ms. And because I don't know ahead of time how long the
# messages might be, this needs to be always set to 300 ms (for example, setting it to some t)

# One hack around this could be to recognise which commands send back more information and then adjust the
# timeout setting of the serial port for those specific commands...

ser = serial.Serial('/dev/cu.usbmodem0045711',timeout=0.3)
```

```python
ser.write(b'?')
s = ser.read(1000).decode("utf-8")
print(s)
```

```python
def query(s):
    ser.write((s+"?").encode("utf-8"))
    return ser.read(1000).decode("utf-8")

def write(s):
    ser.write(s.encode("utf-8"))
    
def set_check(s):
    write(s)
    return query(s[0])
```

```python
set_check("f3900.000")
```

```python
set_check("o1")
```

```python
set_check("t1.0")
```

```python
set_check("s5.0")
```

```python
set_check("g1")
```

```python
set_check("j1")
```

```python
for d in 1,2,3:
    set_check('a%d' % d)
    time.sleep(0.1)
```

```python
print(query(""))
```
