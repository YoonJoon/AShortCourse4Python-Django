<!-- $theme: gaia -->

review2
=======

###### Created by [이 윤 준](https://www.facebook.com/yoonjoon.lee) (yoonjoon.lee@gmail.com)

May, 2019

---

1.	다음은 Calculator 클래스이다.

```python
class Calculator:
    def __init__(self):
        self.value = 0

    def add(self, val):
        self.value += val
```

위 클래스 Calculator로 부터 상속하는 UpgradeCalculator를 만들고 값을 뺄 수 있는 minus 메서드를 추가하시오. 즉, 다음과 같이 동작하는 클래스를 만드시오.

```python
cal = UpgradeCalculator()
cal.add(10)
cal.minus(7)

print(cal.value)  # 10에서 7을 뺀 3을 출력
```

---

2. 다음 코드의 실행결과를 예측하고 그 이유에 대해서 설명하시오.

```python
result = 0
try:
    [1, 2, 3][3]
    "a"+1
    4 / 0
except TypeError:
    result += 1
except ZeroDivisionError:
    result += 2
except IndexError:
    result += 3
finally:
    result += 4

print(result)
```
