```sql
Error Code: 1055. Expression #1 of SELECT list is not in GROUP BY clause 
and contains nonaggregated column 'scd_benchmark.script_polish.unique_group' 
which is not functionally dependent on columns in GROUP BY clause; 
this is incompatible with sql_mode=only_full_group_by
```

GROUP BY 문법은 맞게 작성되었는데 위와 같은 오류가 발생하였다. 

[MySQL 5.7 Reference Manual > 12.20.3 MySQL Handling of GROUP BY](https://dev.mysql.com/doc/refman/5.7/en/group-by-handling.html
)
위 링크를 참고하면, "MySQL 5.7.5 and later implements detection of functional dependence. If the `ONLY_FULL_GROUP_BY` SQL mode is enabled (which it is by default), MySQL rejects queries for which the select list, HAVING condition, or ORDER BY list refer to nonaggregated columns that are neither named in the GROUP BY clause nor are functionally dependent on them"

요약하면, `ONLY_FULL_GROUP_BY` SQL mode가 mysql에서 `enabled` 상태로 되어 있어서, GROUP BY에 있지 않은 nonaggregated column이 참조되는 query문을 거부하는 것으로 보인다.

이를 해결하기 위한 방법은 크게 두 가지로 나뉘는 것 같다.

## 1. Query문 수정

standard SQL query문을 작성한다.

```sql
SELECT unique_group, change_id FROM script_polish
WHERE case_of_Recently BETWEEN 1 AND 7
GROUP BY change_id
HAVING (COUNT(DISTINCT(participant_num)) >= 2)
	AND (COUNT(DISTINCT(case_of_Recently)) >= 2);
```

* 위 코드가 내가 작성한 SQL문인데, GROUP BY에 선언되지 않은 `unique_group` column을 호출하려고 하니 오류가 발생한다.
* 따라서, `unqiue_group` column을 GROUP BY에 추가하니 정상적으로 query문이 실행된다.

## 2. SQL mode set

이 방법은, `sql_mode`에 설정되어 있는 `ONLY_FULL_GROUP_BY` 속성을 끄는 것이다.

이 방법은 다음과 같다.

```sql
/* 현재 설치된 sql_mode의 값을 확인한다 */
mysql> SHOW VARIABLES LIKE 'SQL_MODE'; 

/* 기존의 값들을 복사한 뒤, ONLY_FULL_GROUP_BY 속성만 제외하고 다시 setting해준다 */
SET SESSION sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
```


### 결론: 개인적인 생각으로는, 1번 내용처럼 query문을 standard하게 작성하는 것이 좋은 방법으로 보인다. 
