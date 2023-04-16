# To do list



### 1. Primary Key(기본 키) 설정

* Natural Key
  * 비즈니스에 의미가 있는 키
  * e.g) 이메일, 주민등록번호 등
* Surrogate Key
  * 비즈니스와 관련 없는, 임의로 만들어진 키
  * e.g) MySQL Auto Increment, identity, Oracle sequence, etc.

* 개인적인 생각으로는, 구글링을 통해 여러 블로그를 참고한 결과, Natural Key의 경우 변경 가능성이 있다. 특히 주민등록번호의 경우, 변경되지 않을 것으로 보이지만 수백만의 사용자 중 주민등록번호가 변경되는 사람이 없을 것으로 장담하기 힘들다. 따라서 Primary Key는`Surrogate Key`를 사용하는 것이 권장된다. 



#### 1-1. 소셜 로그인(Google OAuth, KaKao 등)

> [sns로그인 db관리 질문](https://okky.kr/questions/433730)

이 경우 테이블을 어떻게 설계할 것인가. 추후 생각하기





### 2. Database 결정: RDB vs NoSQL

> https://www.baeldung.com/hibernate-ogm
>
> https://www.baeldung.com/mongodb-morphia



#### NoSQL: AWS DynamoDB

* JPA와 NoSQL 매핑
  * Hibernate-OGM: 2018년 이후로 업데이트가 되지 않았으며, [공식 사이트](https://hibernate.org/ogm/) 에는 `Hibernate OGM is not maintained anymore.` 라는 문구가 적혀 있다. 
  * Morphia: MongoDB Object Document Mapping for the JVM.



#### RDB: AWS RDS

* Amazon Aurora is a part of Amazon RDS(공식 문서)

* JPA와 RDB 매핑





### 100. 기타

1. 실무에서는 Table에 id를 `table명_id` 이런 식으로 작성하는 것이 관례라고 한다. id의 경우 여러 테이블에서 사용될 수 있고 많은 경우 Primary Key로 사용되므로, `table명_id` 로 작성하는 것이 내가 생각하기에도 더 괜찮은 것 같다. 
   * e.g) `orders table`의 `orders_id column`, ` members table`의 `member_id column`, ...
