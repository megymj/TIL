# MySQL Data 백업, 복원

SQL Query를 작성 중에 Query문을 잘못 작성해서, 약 100개의 row가 날아갔다.   
이후 구글링을 통해 실행한 Query를 취소하는 방법을 찾아보았으나, 안타깝게도 실행을 취소하는 방법은 존재하지 않는 것으로 보였다.   
따라서, 앞으로 복잡한 작업을 할 때에는 적절한 시점에 data를 백업하면서 진행하려고 한다. 

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
```

