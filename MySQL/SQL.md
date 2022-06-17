> reference link: [w3schools-MySQL](https://www.w3schools.com/mySQl/mysql_sql.asp)

# SQL 유용한 문법 정리

## 1. CREATE TABLE

* Table1과 같은 구조의 table 만들기(data는 생성되지 않는다 )

  ```sql
  CREATE TABLE [table_name] LIKE Table1;
  ```

* Table1과 같은 구조, 같은 데이터의 table 만들기

  ```sql
  CREATE TABLE [table_name] AS SELECT * FROM Table1;
  ```

  * **Precautions**: `CREATE ~ AS`를 사용할 때, 데이터 구조는 동일하게 저장되지만, index(PRIMARY KEY, FOREIGN KEY 등)의 정보는 저장되지 않는다.



## 2. TRUNCATE verses DELETE FROM

* TRUNCATE

  * 전체 데이터를 한 번에 삭제하는 방식
  
  * Auto Increment가 1부터 시작됨
* DELETE FROM
  * WHERE절을 이용하여 원하는 데이터만을 삭제하는 방식
  
  * Auto Increment가 데이터가 지워진 내역 바로 다음부터 시작됨(ex. id가 1~5까지인 data가 지워졌으면, DELETE FROM 이후 id에 6부터 들어감)


## 3. ORDER BY RAND() LIMIT 



## 4. Transaction 


