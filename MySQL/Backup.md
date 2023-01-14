# MySQL Data 백업, 복원

SQL Query를 작성 중에 Query문을 잘못 작성해서, 약 100개의 row가 날아갔다.   
이후 구글링을 통해 실행한 Query를 취소하는 방법을 찾아보았으나, 안타깝게도 실행을 취소하는 방법은 존재하지 않는 것으로 보였다.   
따라서, 앞으로 복잡한 작업을 할 때에는 적절한 시점에 data를 백업하면서 진행하려고 한다. 



## 방법1] mysqldump

<br>

## 예시로 사용할 데이터베이스 db, 계정 정보
```
DB Name: test1_db
table: test1_table
사용자 계정: user
password: 1234
```

### 1. MYSQL DB 백업하기
```SQL
# 명령어
mysqldump -u [사용자 계정] -p [원본 데이터베이스명] > [생성할 백업 DB명].sql

# 사용 예시
mysqldump -u user -p test1_db > backup_test1_db.sql
password: 1234
```

### 1-1. MYSQL DB 복원하기
```SQL
# 명령어
mysqldump -u [사용자 계정] -p [복원할 DB] < [백업된 DB].sql

# 사용 예시
mysqldump -u user -p test1_db < backup_test1_db.sql
password: 12

# 위의 예시가 작동되지 않는 경우 
mysql -u user -p test1_db < backup_test1_db.sql
password: 1234
```

### 2. MYSQL TABLE 백업하기
```SQL
# 명령어
mysqldump -u [사용자 계정] -p [데이터베이스명] [원본 백업받을 테이블명] > [백업받을 테이블명].sql

# 사용 예시
mysqldump -u user -p test1_db test1_table> backup_test1_table.sql
password: 1234
```

### 2-1. MYSQL TABLE 복원하기
```SQL
# 명령어
mysqldump -u [사용자 계정] -p [복원할 DB ] < [백업된 테이블].sql

# 사용 예시
mysqldump -u user -p test1_db < backup_test1_table.sql
password: 1234

# 위의 예시가 작동되지 않는 경우 
mysql -u user -p test1_db < backup_test1_table.sql
password: 1234
```

<br>



## 3. mysqldump로 table 복원이 되지 않는 경우

> reference link: https://www.phpschool.com/gnuboard4/bbs/board.php?bo_table=qna_db&wr_id=199595

백업된 .sql 파일이 위치한 디렉토리로 이동한 뒤, 

```sql
mysql -u root[혹은 user name] -p [database name] -e "source filename.sql"
```

와 같이 입력하니, 정상적으로 백업되는 것을 확인하였다.



mac] localhost에 backup

```sql
# localhost 뒤에 port number 붙이면 오류 발생함
mysql -h localhost -u root -p [database name] -e "source [filename].sql"
```





AWS RDS의 경우도 마찬가지로 수행하니 잘 작동됨. 백업된 .sql 파일이 위치한 디렉토리로 이동한 뒤,

```sql
mysql -h [RDS Endpoint] -u [user name] -p [database name] -e "source [filename].sql"
```



#### Database backup도 마찬가지 방식으로 수행

* 단, backup할 database를 미리 만들어 놓고, 지정해 줘야함. [database name] 이 빠지면 query문 오류 발생함

```sql
mysql -h [RDS Endpoint] -u [user name] -p [database name] -e "source database.sql"
```



<br>



## 방법 2] MySQL Workbench에서, Table Data Export Wizard, Table Data Import Wizard
* Table Data Export Wizard 사용 시, 약 9,600개의 row가 포함된 data에 있어서는 10초 안에 Export가 되는 것을 확인함.
* 하지만 다수의 data가 table에 존재하는 경우에는, `적합하지 않다`



