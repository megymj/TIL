# sql background에서 실행하기(ubuntu 20.04)

sql command 실행 시간이 너무 긴 경우, background에서 실행하기 위해 여러 가지 방법을 찾아봄.



<br>



## 방법1. linux screen command 사용

* ubnutu server에서 screen 새로 생성

* mysql 접속 이후, sql 파일을 실행한다

  ```bash
  mysql > source /home/mjkim/test.sql;
  ```

* screen을 빠져 나온다



<br>



## 방법2. nohup과 & 사용

> reference link: [stackoverflow: running a sql query in the background](https://stackoverflow.com/questions/30224105/running-a-sql-query-in-the-background-using-mysql)

```bash
nohup mysql <options> -u <user> -p <pass> -e 'Insert Query Here' &
```

- `nohup` is for 'no hang up' i.e. don't end the process when I disconnect.
- `mysql ...`The mysql command you usually run to connect, just add the -e option to run the query without opening a mysql session
- `&` starts the process in the background
- "nohub.out" file에서 로그 확인 가능



### Process 종료

```bash
kill -9 PID
```




