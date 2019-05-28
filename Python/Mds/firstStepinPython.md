<!-- $theme: gaia -->

[The First Step in Python](https://github.com/YoonJoon/ProgramminginPython/blob/master/Part1/intro_Ex.ipynb)
============================================================================================================

###### Created by [이 윤 준](https://www.facebook.com/yoonjoon.lee) (yoonjoon.lee@gmail.com)

May, 2019

---

### Overview

<br>

구문과 자료 구조를 소개하기 위하여 작은 Python 프로그램을 작성하고 설명

---

### White Noise Process

<br>

###### ε0,ε1,…,εT를 시뮬레이션하고 플롯 (각 εT는 독립적인 정규분포)

![](https://github.com/YoonJoon/ProgramminginPython/raw/366dc77c4c60556357f912ac7c687db479ebd553/Part1/Figures1/pythonfig1-3-1.png)

---

### 1st Version

<br>

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.random.randn(100)
plt.plot(x)
plt.show()
```

---

### 2nd Version

<br>

```python
import numpy as np
import matplotlib.pyplot as plt

ts_length = 100
epsilon_values = []   # Empty list

for i in range(ts_length):
    e = np.random.randn()
    epsilon_values.append(e)

plt.plot(epsilon_values, 'b-')
plt.show()
```

---

### List

<br>

```python
x = [10, 'foo', False]  # We can include heterogeneous data inside a list
type(x)
```

```python
x
```

```python
x.append(2.5)
x
```

```python
x[1]
```

#### List Comprehension

```python
animals = ['dog', 'cat', 'bird']
plurals = [animal + 's' for animal in animals]
plurals
```

---

### <code>For</code> Loop

<br>

```python
animals = ['dog', 'cat', 'bird']
for animal in animals:
    print("The plural of " + animal + " is " + animal + "s")
```
