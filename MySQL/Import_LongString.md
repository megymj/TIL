# MySQL WorkBench에서

.csv 파일을 Table import wizard로 가져오려고 하는데, 

csv 파일에서 특정 컬럼 내부에 있는 항목이 엄청 긴 경우(문자열 등)

예)
![image](https://user-images.githubusercontent.com/80478750/166926399-c2d6ca63-ec17-419b-99ba-0d23b5003be9.png)


* 위와 같은 경우, workbench에서 Table import wizard를 실행하면, Column을 제대로 읽어오지 못하는 오류(컬럼을 중복해서 읽어버리는)가 발생한다. 'a'와 'b' column의 내용을 "Hello" 처럼 단순한 문자열로 변경하니, 정상적으로 Column을 읽어오는 것을 확인했다.
* pymysql을 사용하여 넣으려 해도 오류가 발생하였다. 
  * 1064, "You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ...

<br>

## pymysql을 사용해서 오류 해결을 시도함!

### 시도 1

* 문자열 사이의 공백을 없애봄

  ```python
  Type = Type.replace(" ", "")
  a = a.replace(" ", "")
  Type2 = Type2.replace(" ", "")
  b = b.replace(" ", "")
  ```

* 결과: 여전히 동일한 오류 발생




### 시도 2

* 원인 해결: Column 명이 문제였다. pymysql에서, table의 컬럼 명으로 설정한 것 중 'Group', 'Type'이 있었는데(위 사진 참고), 이 두 keyword가 알고보니 예약어였다. 
* 따라서, Group -> gr, Type -> Type1으로 변경한 뒤, 시도하니 정상적으로 table에 들어가는 것을 확인할 수 있었다. 
* 시도 1에서, 문자열 공백을 없앤 것과는 상관없음.



### 결론

* `MySQL에서, table명을 지을 때 예약어는 무조건 피하자`



### 의문점

* 위에 사진에서, workbench의 table import wizard 기능에서는, table의 컬럼 명이 예약어임에도 불구하고, 내용을 줄이니 정상적으로 인식한 반면, pymysql에서는 내용을 줄여도("Hello", "Hi" 등의 문자열로 변경해봄) 동일한 오류가 발생하면서 db에 넣어지지 않았다.
* 반대로, table import wizard 에서는 시도 2에서 column 명을 변경한 .csv 파일을 읽어오게 되면, 여전히 컬럼이 중복되는 문제가 발생한다.
* `정리하면, table import wizard에서는 매우 긴 문자열을 인식하지 못하고, pymysql에서는 예약어를 인식하지 못하는 것으로 보인다.`
