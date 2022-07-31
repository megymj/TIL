# 많은 개수의 row 값 update 하기



Situation]

Table 1과 Table 2에서, **Table 1의 'file_path' column을 varchar 형식이 아닌, Table 2의 file_path_id의 값으로 변경한 뒤**, 최종적으로 Table 1의 file_path와 Table 2의 file_path_id를 join하여 경로를 보고자 함.

'file_path'의 data를 varchar에서 int로 변환하는 이유는, 이후 .csv 파일로 data를 다운받았을 때, 용량을 줄이기 위함임.

Table 1의 data 개수: 약 23,000,000개



Table 1

| Field     | Type         | Key  | data example                                                 |
| --------- | ------------ | ---- | ------------------------------------------------------------ |
| id        | int          | PRI  | 1                                                            |
| commit_id | int          |      | 1307827                                                      |
| file_path | varchar(500) |      | qa/os/src/test/java/org/elasticsearch/packaging/util/ServerUtils.java |



Table 2

| Field        | Type         | Key  | data example                                                 |
| ------------ | ------------ | ---- | ------------------------------------------------------------ |
| file_path_id | int          | PRI  | 1                                                            |
| file_path    | varchar(500) |      | qa/os/src/test/java/org/elasticsearch/packaging/util/ServerUtils.java |

<br>

## 공통

먼저, Table 1을 통해 Table 2를 생성함.

* Table 2의 data 개수: 약 140,000개

```SQL
/* table 2 구조 생성 */
CREATE TABLE table 2... 

/* 기존의 table 1에 있는 'file_path' column 중, distinct한 value를 table2로 삽입. table 2의 file_path_id column은 PK, AL 설정이 되어 있어서, 자동으로 update됨 */
INSERT INTO table1 (file_path)
SELECT distinct(file_path)
FROM table2;
```

<br>

## 시도 1. procedure 사용

```sql
DELIMITER $$
CREATE PROCEDURE WHILEPROC() 
BEGIN
/* table1의 file_path 를 VARCHAR 에서 int로 수정하기 위한 procedure */
		DECLARE i INT DEFAULT 1;
    DECLARE iid INT;
    DECLARE pathh VARCHAR(500);
    DECLARE lengthh INT;
    SET lengthh = (SELECT COUNT(*) FROM table2);
    
    WHILE (i <= lengthh) DO
    
		 SET iid = ( SELECT file_path_id FROM table2 WHERE file_path_id = i);
		 SET pathh = (SELECT file_path  FROM table2 WHERE file_path_id = i);
        
     UPDATE table1 SET file_path = iid WHERE file_path =  pathh;
        
     SET i = i + 1;
    
    END WHILE;

END 
$$
DELIMITER ;

set sql_safe_updates = 0;
call WHILEPROC();
set sql_safe_updates = 1;
```

* 시도 결과: ubuntu 20.04에서 screen으로 돌렸으나, 144시간 동안 전체 23,000,000 column 중 약 1/5정도만 update 되었음 -> 적절한 방법이 아님



<br>



## 시도 2. index 사용

Indexes are used to find rows with specific column values quickly. Without an index, MySQL must begin with the first row and then read through the entire table to find the relevant rows. The larger the table, the more this costs. If the table has an index for the columns in question, `MySQL can quickly determine the position to seek to in the middle of the data file without having to look at all the data`. This is much faster than reading every row sequentially. [mysql documents: How MySQL Uses Indexes](https://dev.mysql.com/doc/refman/8.0/en/mysql-indexes.html)



```sql
/* index 생성 */
CREATE INDEX index1 ON table1 (file_path);

/* data를 insert 할 새로운 table 생성(file_path column이 int형인 것만 제외하면, table1	과 동일한 구조) */
CREATE TABLE table3 {
	...
};

/* data insert */
INSERT INTO table3 (id, commit_id, file_path)
SELECT a.file_id, a.commit_id, b.file_path_id
FROM table1 AS a
INNER JOIN table2 AS b
ON a.file_path = b.file_path;
```

* 결과: 'Query OK, 23000000 rows affected (6 min 7.52 sec)'
* 처음에 procedure는 기존 table의 data를 수정, index를 사용할 때는 새로운 table에 data를 insert하는 방식으로 방식은 달랐으나, 그럼에도 불구하고 실행 시간 차이가 매우 큰 것을 확인할 수 있었다. 
* 결론: data row가 많은 경우 MySQL index를 활용하자.

