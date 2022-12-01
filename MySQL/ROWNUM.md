# ROWNUM

> 참고 링크
>
> [rownum을 사용해서 번호 매기기](https://developer-jjun.tistory.com/23)
>
> [rownum으로 번호 매기기, 역순 포함](https://baessi.tistory.com/104)
>
> [rownum 초기화](https://blog.naver.com/PostView.naver?blogId=hj_kim97&logNo=222215389986&redirect=Dlog&widgetTypeCall=true&directAccess=false)



### 1. ROWNUM 이란

Oracle에서 사용되는, 각 테이블에 행 번호를 매길 수 있는 방법이다. 특정 집합에서 원하는 만큼의 행만 가져오고 싶을 때, 행의 개수를 제한하는 용도로 사용된다. 

jsp-servlet 실습에서는, 게시판에서 특정 개수의 글만큼을 가져올 때 사용하였다. 

* "Row의 id값을 기준으로 가져오면 되지 않냐" 라고 생각할 수 있으나, 사용자들이 게시글을 삭제할 수도 있으므로, id값을 기준으로 하게 되면 1, 7, 8, 11, ... 등과 같이 id값이 띄엄띄엄 존재하게 될 수 있다. 





### 2. MySQL 에서 ROWNUM 사용하기

MySQL 혹은 MariaDB 에서는 ROWNUM을 지원하지 않기 때문에 '@' 를 통해 변수를 생성하여 사용한다. 

변수의 초기화는 FROM, WHERE 절에서 사용한다.







#### 2-1. MySQL 에서 ROWNUM을 사용하는 예시

* 아래 Table은 예시로 사용한 notice Table

| **id** | **title**                       | **writer_id** | **content**                    | **regdate**         | **hit**  | **files**                | **pub** |
| ------ | ------------------------------- | ------------- | ------------------------------ | ------------------- | -------- | ------------------------ | ------- |
| **1**  | JDBC란 무엇인가?                | okay          | aaa                            | 2022-10-18 11:28:36 | 23151465 | test.zip, aa.gif, bb.png | 0       |
| **2**  | JDBC 드라이버 다운로드 방법     | newlec        | aaa                            | 2022-10-18 11:28:36 | 10       |                          | 0       |
| **3**  | 전화주시기 바랍니다.            | newlec        | 연락처 남깁니다. 010-2222-2333 | 2022-10-18 11:29:00 | 22       |                          | 0       |
| **4**  | Service 계층 추가하기           | dragon        | aaa                            | 2022-10-18 11:29:00 | 1        |                          | 0       |
| **5**  | JSP에서 JDBC 사용하기           | newlec        | soso                           | 2022-10-18 11:31:12 | 33       |                          | 0       |
| **6**  | 파라미터를 가지는 문장 실행하기 | okay          | aaa                            | 2022-10-18 11:31:12 | 1        |                          | 0       |
| **7**  | 추가요                          | dragon        | aaa                            | 2022-10-18 11:31:12 | 44       |                          | 0       |
| **8**  | 디엔드                          | okay          | aaa                            | 2022-10-18 11:31:12 | 55       |                          | 0       |
| **9**  | test                            | newlec        | test content                   | 2022-11-03 11:36:42 | 0        |                          | 0       |
| **10** | test2                           | newlec        | test content2                  | 2022-11-03 11:58:46 | 0        |                          | 0       |



> sql문 설명 링크: [유튜브 뉴렉처: JSP 강의 72~74](https://www.youtube.com/watch?v=xFGkI3SuJ-U&list=PLq8wAnVUcTFVOtENMsujSgtv2TOsMy8zd&index=73)

```sql
/* 1. SET구문을 사용하여 ROWNUM 값을 초기화 후 조회 */
 SET @ROWNUM := 0;
 SELECT @ROWNUM := @ROWNUM + 1 AS rownum,  notice.* FROM notice;


/* 2-1. FROM 절에서 초기화 후 조회 예시1 */
SELECT * FROM (
	SELECT @ROWNUM := @ROWNUM + 1 AS ROWNUM, N.*
	FROM (SELECT * FROM notice ORDER BY REGDATE DESC) AS N
) AS A, (SELECT @ROWNUM := 0) AS B
WHERE (ROWNUM BETWEEN 6 AND 10); 

/* 2-2. FROM 절에서 초기화 후 조회 예시2 */
SELECT @ROWNUM := @ROWNUM + 1 as ROWNUM, board.* 
FROM board, (SELECT @ROWNUM := 0) AS R
WHERE id = any ( 
	SELECT id FROM board 
    WHERE regdate > (SELECT regdate FROM board WHERE id = 3 )
)
ORDER BY @ROWNUM ASC
LIMIT 1;
```



#### 2-2. Subquery 오류 설명

위의 2-2 sql문에서, Subquery에서 "any"를 제외하게 되면 "Error Code : 1242. Subquery returns more than 1 row" 메시지가 출력되며 오류가 발생한다. 

해당 오류의 원인은, 말 그대로 서브쿼리의 결과가 여러 개가 반환이 되기 때문이다. 

따라서 `any` 를 앞에 붙여서 오류를 해결한다. 

* 번외로] 예시 Table에서, regdate가 두 번째로 큰 것의 id=9이므로, 2-2 query에서 id = 3 -> id = 9로 수정하고 실행하니 1242 Error 코드가 발생하지 않고 한개의 결과 row가 정상적으로 출력된 것을 확인할 수 있었다. 

