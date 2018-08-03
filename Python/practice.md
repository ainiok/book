## 一元二次方程解
```
import math


def quadratic(a, b, c):
    if not isinstance(a, (int, float)):
        raise TypeError('a is not a number')
    if not isinstance(b, (int, float)):
        raise TypeError('b is not a number')
    if not isinstance(c, (int, float)):
        raise TypeError('c is not a number')
    d = b * b - 4 * a * c
    if a == 0:
        if b == 0:
            if c == 0:
                return '方程根为全体实数'
            else:
                return '方程无根'
        else:
            x1 = -c / b
            x2 = x1
            return x1, x2
    else:
        if d < 0:
            return '方程无根'
        else:
            x1 = (-b + math.sqrt(d)) / 2 / a
            x2 = (-b - math.sqrt(d)) / 2 / a
            return x1, x2
```


## ……