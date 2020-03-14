<!--
.. title: Monte Carlo Estimation of pi
.. slug: monte-carlo-estimation-of-pi
.. date: 2020-03-14 21:53:19 UTC+01:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# Monte Carlo estimation of Pi


```python
import random

import numpy as np
from numba import jit
```

# Comparing three different implementations
    1 - using python built-in random and sum
    2 - vectorization using numpy
    3 - Using numbas's jit: translating python code to optimized machine code at runtime 


```python
def dart_board():
    x,y = random.random(), random.random()
    return (x**2 + y**2 <= 1.0)

def pi(n_samples):
    s = 4.0 * sum([dart_board() for _ in range(n_samples)]) / n_samples

def np_pi(n_samples):
    x = np.random.random(n_samples)
    y = np.random.random(n_samples)

    return 4.0 * (x**2 + y**2 <= 1.0).sum() /n_samples

    
@jit(nopython=True)
def jit_pi(n_samples):
    acc = 0
    for i in range(n_samples):
        x = random.random()
        y = random.random()
        if (x ** 2 + y ** 2) < 1.0:
            acc += 1
    return 4.0 * acc / n_samples
```


```python
n_samples = 10000
```


```python
%timeit np_pi(n_samples)
```

    180 µs ± 2.09 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)



```python
%timeit pi(n_samples)
```

    4 ms ± 92.1 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)



```python
timeit jit_pi(n_samples)
```

    126 µs ± 1.89 µs per loop (mean ± std. dev. of 7 runs, 1 loop each)

