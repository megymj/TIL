# MacOS python3 설치 및 PyCharm 버전 변경



기존에 MacOS는 Python3.9.6 버전이 자동으로 설치되어 있었으며, 아래 메시지로 확인 가능하다

```bash
python -V	# 이 메시지는 정상 응답이 출력되지 않음
python3 -version	# Python 3.9.6
```



그런데 PyCharm에서 `PyGitHub`, `request` lib을 사용하려 하였으나, 아래와 같은 오류가 발생하였다. 

```
ImportError: urllib3 v2.0 only supports OpenSSL 1.1.1+, currently the 'ssl' module is compiled with LibreSSL 2.8.3.
```



https://github.com/urllib3/urllib3/issues/2168 링크를 참고하니, Python 버전을 3.10 이상을 사용해야 하는 것으로 보였다. 



## MacOS Python 3.10 설치

### 시도 1. pyenv 사용

```bash
brew install pyenv # Install pyenv
pyenv install --list # Check version
pyenv install 3.10.8

# 이후 ~/.zshrc 파일 내에 설정을 해 줘야 하는 것 같으나, 따로 작업하지 않았음
# 위의 설치한 python 3.10.8 버전은 제거하였음
```



### 시도 2. 공식 홈페이지에서 설치

https://www.python.org/ 에서 3.10.10 버전을 설치하였다.

```bash
# 설치 위치 확인
python
>>> import sys
>>> sys.executable
'/usr/local/bin/python'
```

`/usr/local/bin` 으로 이동하니, Python3.10 버전이 위치한 것을 확인하였다.

이후 Pycharm > New Project > Base interpreter에서, `/usr/local/bin/python3.10` 을 선택한 뒤에 `PyGitHub`, `request` lib을 사용하니 정상적으로 실행이 되는 것을 확인하였다. 