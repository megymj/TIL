# MySQL] .sql 결과를 .csv 파일로 저장: INTO OUTFILE

> reference link: [scp command in linux](https://phoenixnap.com/kb/linux-scp-command)

* 상황] `MySQL Workbench > Data Export Wizard` 를 사용해서 table data를 .csv파일로 저장하려고 하였으나, row의 개수가 약 1,500,000개 정도가 들어있는 것을 읽어 저장하려고 하니 시간이 매우 많이 걸리며 또한 "응답 없음" 오류가 발생하여 다른 방법을 찾기로 하였다.
* 따라서, MySQL query 중`INTO OUTFILE` command를 통해 결과를 서버에 저장한 뒤, Filezilla 등의 방법을 통해 로컬로 불러오고자 하였다.

```sql
# command
SELECT * FROM commits
INTO OUTFILE '/var/lib/mysql-files/commits.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n';

# copy & paste 용도
SELECT * FROM files_copy INTO OUTFILE '/var/lib/mysql-files/files_copy.csv' FIELDS TERMINATED BY ',' ENCLOSED BY '"' LINES TERMINATED BY '\n';
```

* 하지만 위에서 `INTO OUTFILES` command 사용 시, 아래와 같은 오류가 발생한다.

  ```
  Error Code: 1290. The MySQL server is running with the --secure-file-priv option so it cannot execute this statement
  ```

  오류 해결 참고 링크: [tistory blog](https://myinfrabox.tistory.com/209)

* 이후 /var/lib/mysql-files 경로에 .csv 파일을 저장하였다.



### local로 파일 전송

1. FileZilla client 사용: /var/lib/mysql-files 접근 권한이 없어서 실패.

2. scp(Secure Copy Protocol) command 사용:

   * linux mv command를 사용하여 .csv 파일 위치를 /home/mjkim 으로 이동

   * terminal 에서, `scp -P 포트번호 username@원격서버주소:해당경로폴더/해당파일명 로컬저장소위치` command를 입력

   * 파일에 Permission denied error 발생 -> root 계정으로 chown command를 통해 소유자 변경
   
   * 이후 위의 scp command를 입력하니, 정상적으로 저장이 됨 (로컬 저장소 위치: ~/Desktop/[Directory name] : Mac 기준) 



<br>



### +a: SELECT ... INTO Statement

> reference link: [MySQL documents](https://dev.mysql.com/doc/refman/8.0/en/select-into.html)

- `SELECT ... INTO  var_list` selects column values and stores them into variables.
- `SELECT ... INTO OUTFILE` writes the selected rows to a file. Column and line terminators can be specified to produce a specific output format.
- `SELECT ... INTO DUMPFILE` writes a single row to a file without any formatting.
