> reference link: [1. MySQL에서 csv 파일 import하기](https://cotak.tistory.com/63), [2. MySQL에서 csv 파일 export하기](https://jihunworld.tistory.com/46)

# MySQL WorkBench 사용 Tip

<br>

## 1. MySQL에서 csv 파일 import하기
MySQL은 '.csv'파일 양식으로 불러와야 한다. '.xlsx' 양식 불러오기 불가능   

<br>

## 2. MySQL에서 csv 파일 export하기
**Precautions]** Field Seperator를 `';'` 이 아니라 `','`로 바꿔줘야 정상적으로 column이 잘 나뉜다.

<br>

## 3. query문 없이 직접 수정하기
> reference: google에 `Mysql Workbench read only` keyword로 검색
```sql
SELECT * FROM TABLE1
```
* 처럼 `*`로 조회한 경우에만 가능. 또한 Table 내에 PK가 있어야 함
* Table 내부를 마우스로 직접 클릭해서 값 수정이 가능함







