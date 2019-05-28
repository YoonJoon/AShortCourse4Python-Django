<!-- $theme: gaia -->

Setting Up Development Environment for Python
=============================================

###### Created by [이 윤 준](https://www.facebook.com/yoonjoon.lee) (yoonjoon.lee@gmail.com)

May, 2019

---

### Install Python3

1.	필수 설치 프로그램을 다운로드하십시오.

	1.	https://www.python.org/downloads/ 로 이동하십시오.
	2.	Download Python 3.6.X download 하십시오 (정확한 마이너 버전 번호는 다를 수 있음).

2.	다운로드한 파일을 두 번 클릭하고 설치 프롬프트에 따라 Python을 설치하십시오.

3.	"Add Python to PATH" 상자 선택을 확인하십시오

###### 명령 프롬프트에서 아래 텍스트를 입력하여 Python 3 설치를 확인

```bash
py -3 -V
Python 3.6.X
```

---

### Install pip3 (Python Package Manager)

<br>

###### i) 환경변수 PATH에 <code>C:\\Program Files\\Scripts</code>를 추가

```bash
set PATH "%PATH;C:\\Program Files\\Scripts"
```

###### ii) cmd 창을 닫고 다시 실행하여 PATH 확인

```
echo "%PATH%"
"... C:\Program Files\Python36\;C:\Program Files\Python36\Scripts\ ..."
```

###### iii) pip3 설치 확인

```
pip3 list
```

###### iv) 필요하면 pip3 업데이트

```
py -3 -m pip install --upgrade pip
```

---

### Set Up a Virtual Environment on Windows

<br>

###### 명령 프롬프트에서 다음 명령을 실행

```bash
pip3 install virtualenvwrapper-win
```

###### 명령 프롬프트에서 가상환경 생성

```bash
$ mkvirtualenv my_python36
.
.
.
(my_python36) C:\>
```

---

### Useful Commands to Use a Virtual Enviornment

<br>

-	###### <code>deactivate</code> - 현재 파이썬 가상 환경을 종료

```
> Envs\my_python36\Scripts\deactivate
```

-	###### <code>workon</code> - 사용 가능한 가상 환경 나열
-	###### <code>workon name_of_environment</code> - 지정된 Python 가상 환경 활성화
-	###### <code>rmvirtualenv name_of_environment</code> - 지정된 환경을 제거합니다.

---

### See also

<br>

[Python3 설치](https://github.com/YoonJoon/AboutDjango/blob/master/developmentEnvironment.md)

