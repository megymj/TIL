# MySQL 사용 관련 오류 정리

## 1. MySQL WorkBench

### unhandled exception 'ascii' codec can't encode character in position ordinal not in ....
* MySQL Workbench로 'Table Data Export Wizard'를 사용할 때 발생한 오류
* 해당 오류의 원인은 구글링한 결과 크게 두 가지로 나뉘는 것 같다.
  1. 파일의 절대 경로에 `한글`이 들어간 경우
  2. 파일 명에 `한글`이 포함된 경우
* 해결 방법
  * 1, 2번에서, `한글` 부분을 영어로 변경해주면 정상적으로 잘 작동한다. 



## 2. 기타

### AttributeError: partially initialized module 'pymysql' has no attribute 'connect' (most likely due to a circular import)
* python에서, pymysql library 사용 시 발생한 오류
* 해당 오류의 원인: library명과 파일명이 동일해서 발생함(pymysql.py, import pymysql)
