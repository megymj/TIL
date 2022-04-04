> 참고 사이트: [tistory blog](https://cotak.tistory.com/63)

# 파일 읽어오기
MySQL은 .csv파일 양식으로 불러와야 한다.(.xlsx 양식 불러오기 불가능)

<br>

## 서버의 mysql에 local(내 컴퓨터)의 데이터를 집어 넣는 방법
서버에는 Ubuntu 20.04 OS가 설치되어 있음

### 1. 서버에 직접 local의 데이터를 보낸 뒤, 이후 mysql로 mysqldump 등의 방법을 사용해서 집어넣는 방법
시도는 해 보지 않았으나, 어려울 것으로 예상됨

### 2. local의 MySQL Workbench를 이용하는 방법
local에 MySQL Workbench를 설치한 뒤, 서버 mysql의 계정으로 MySQL Connections를 등록한다. 이후 MySQL Workbench 내에서 .csv 파일을 직접 import 한다. 
