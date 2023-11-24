---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.5
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

```python
import numpy as np
import matplotlib.pyplot as plt
from io import StringIO
import pandas as pd
```

Copy paste from:

https://ambtenarensalaris.nl/salarisschalen-wetenschappelijk-onderwijs/

```python
salary_scales_string = '''Step	1	2	3	4	5	6	7	8	9	10	11	12	13	14	15	16	17	18
0	2.402	2.402	2.402	2.402	2.402	2.402	2.471	2.818	3.098	2.960	3.974	4.814	5.506	5.784	6.272	6.794	7.362	8.087
1	2.402	2.402	2.402	2.402	2.402	2.402	2.541	2.960	3.247	3.098	4.123	4.955	5.648	5.924	6.445	6.977	7.597	8.343
2	2.402	2.402	2.402	2.402	2.402	2.402	2.679	3.098	3.413	3.247	4.257	5.093	5.784	6.099	6.618	7.168	7.840	8.607
3	2.402	2.402	2.402	2.402	2.402	2.541	2.818	3.247	3.557	3.413	4.391	5.234	5.924	6.272	6.794	7.362	8.087	8.881
4	2.402	2.402	2.402	2.402	2.471	2.611	2.899	3.336	3.703	3.557	4.522	5.366	6.099	6.445	6.977	7.597	8.343	9.168
5	2.402	2.402	2.402	2.402	2.541	2.679	2.960	3.413	3.839	3.703	4.670	5.506	6.272	6.618	7.168	7.840	8.607	9.455
6	2.402	2.402	2.402	2.471	2.611	2.748	3.026	3.483	3.974	3.839	4.814	5.648	6.445	6.794	7.362	8.087	8.881	9.757
7		2.402	2.471	2.541	2.679	2.818	3.098	3.557	4.123	3.974	4.955	5.784	6.618	6.977	7.597	8.343	9.168	10.069
8			2.541	2.611	2.748	2.889	3.171	3.636	4.257	4.123	5.093	5.924	6.702	7.168	7.840	8.607	9.455	10.390
9			2.611	2.679	2.818	2.960	3.247	3.703		4.257	5.234	6.099		7.362	8.087	8.881	9.757	10.721
10				2.748	2.889	3.026	3.336	3.764		4.391	5.366	6.181						'''
```

```python
function_scales_string = '''Step	H2	H1	P	SA	TOIO
0	6.099	6.794	2.541		2.089
1	6.272	6.977	2.960		
2	6.445	7.168	3.098	2.402	
3	6.618	7.362	3.247	2.402	
4	6.794	7.597		2.611	
5	6.977	7.840			
6	7.168	8.087			
7	7.362	8.343			
8	7.597	8.607			
9	7.840	8.881			
10	8.087	9.168			
11	8.343	9.455			
12	8.607	9.757			
13	8.881	10.069			
14		10.390			
15		10.721	'''
```

```python
salary_scales = pd.read_csv(StringIO(salary_scales_string), sep='\t')
function_scales = pd.read_csv(StringIO(function_scales_string), sep='\t')
```

```python
salary_scales
```

```python
function_scales
```

Postdocs start one step above the end of the salary scale for PhDs, plus one additional year for each year after their PhD.

```python
phd = function_scales["P"][0:4].values*1000
postdoc_3_years = salary_scales["10"][3:6].values*1000
print(phd)
print(postdoc_3_years)
```

This much is a little bit clear at least. But what about starting tenure trackers? In principle, they start with the same rule as postdocs. But we try to get them into 12.0 already when they start, which is equivalent to after a 5 year postdoc. 


```python

```
