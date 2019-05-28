<!-- $theme: gaia -->

[Module](https://wikidocs.net/29)
=================================

##### Created by [이 윤 준](https://www.facebook.com/yoonjoon.lee) (yoonjoon.lee@gmail.com)

May, 2019

---

모듈이란 함수나 변수 또는 클래스 들을 모아 놓은 파일입니다.

---

#### Create a Module

<br>

```python
# mod1.py
def add(a, b):
    return a + b

def sub(a, b):
    return a-b
```

위와 같이 add와 sub함수만 있는 파일 mod1.py를 만들고 <code>C:\Users\YoonJoon\Lectures\ </code> 디렉터리에 저장합니다.

---

#### Use Module

```bash
cd C:\Users\YoonJoon\Lectures
python
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

```python
>>> import mod1
>>> print(mod1.add(3, 4))
7
>>> print(mod1.sub(4, 2))
2
```

```python
>>> from mod1 import add
>>> print(mod1.add(3, 4))
7
```

---


```python
from mod1 import add, sub
```

```python
from mod1 import *
```

---


#### if \_\_name\_\_ == "\_\_main\_\_":

```python
# mod1.py 
def add(a, b): 
    return a+b

def sub(a, b): 
    return a-b

print(add(1, 4))
print(sub(4, 2))
```

```python
> python
Type "help", "copyright", "credits" or "license" for more information.
>>> import mod1
5
2
```

---

```python
# mod1.py 
def add(a, b): 
    return a+b

def sub(a, b): 
    return a-b

if __name__ == "__main__":
    print(add(1, 4))
    print(sub(4, 2))

```

```python
>>> import mod1
>>>
```

###### 파이썬의 <b>\_\_name\_\_</b> 변수는 파이썬이 내부적으로 사용하는 특별한 변수명이다.

---

```python
# mod2.py 
PI = 3.141592

class Math: 
    def solv(self, r): 
        return PI * (r ** 2) 

def add(a, b): 
    return a+b 
```

```python
>python
Type "help", "copyright", "credits" or "license" for more information.
>>> import mod2
>>> print(mod2.PI)
3.141592
```

```python
>>> a = mod2.Math()
>>> print(a.solv(2))
12.566368
```

---

###### 1. sys.path.append(모듈을 저장한 디렉터리) 사용

```python
>python
>>> import sys
>>> sys.path.append("C:/doit/mymod")
```

###### 2. PYTHONPATH 환경 변수 사용

```python
>set PYTHONPATH=C:\doit\mymod
>python
>>> import mod2
```