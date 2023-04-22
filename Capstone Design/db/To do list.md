# To do list



### 1. Primary Key(기본 키) 설정

* Natural Key
  * 비즈니스에 의미가 있는 키
  * e.g) 이메일, 주민등록번호 등
* Surrogate Key
  * 비즈니스와 관련 없는, 임의로 만들어진 키
  * e.g) MySQL Auto Increment, identity, Oracle sequence, etc.

* 개인적인 생각으로는, 구글링을 통해 여러 블로그를 참고한 결과, Natural Key의 경우 변경 가능성이 있다. 특히 주민등록번호의 경우, 변경되지 않을 것으로 보이지만 수백만의 사용자 중 주민등록번호가 변경되는 사람이 없을 것으로 장담하기 힘들다. 따라서 Primary Key는`Surrogate Key`를 사용하는 것이 권장된다. 
  * 본 프로젝트에서는 PK를 `id` 와 `e-mail`로 사용하기로 하였으며, 소셜 로그인을 추가하게 된다면 추후 **변동 가능성 있음** 




#### 1-1. 소셜 로그인(Google, KaKao)

> [sns로그인 db관리 질문](https://okky.kr/questions/433730)
>
> https://developers.google.com/identity/protocols/oauth2

로그인/회원가입 직접 구현

* 장점: 구현을 빠르게 할 수 있다. 또한 회원 데이터를 직접 관리할 수 있다.
* 단점: 회원가입 시 이메일 인증, 비밀번호 찾기, 비밀번호 변경, 회원정보 변경, 로그인 시 보안 문제 등을 고려해야 한다.



소셜 로그인

- 장점: 로그인/회원가입 직접 구현 에서의 단점들을 구글, 카카오에 맡기면 되니 서비스 개발에 집중할 수 있다.
- 단점: 해당 부분 학습이 필요함(Oauth)



-> 본 프로젝트에서는, 소셜 로그인을 사용하여 회원가입 기능을 구현하기로 하였음





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



서비스의 주 기능이 챗봇 서비스이므로, NoSQL이 조금 더 적합해 보이지만 공부를 위해 RDB로 시작하기로 하였다. 그리고 RDB(MySQL)의 경우 column type을 TEXT, LONGTEXT 등을 사용할 수 있으며, 이에 따라 대화 내용이 길어도 모두 하나의 컬럼에 들어갈 수 있을 것으로 보인다. 





### 3. AWS RDS 구축







### 100. 기타

1. 실무에서는 Table에 id를 `table명_id` 이런 식으로 작성하는 것이 관례라고 한다. id의 경우 여러 테이블에서 사용될 수 있고 많은 경우 Primary Key로 사용되므로, `table명_id` 로 작성하는 것이 내가 생각하기에도 더 괜찮은 것 같다. 
   * e.g) `orders table`의 `orders_id column`, ` members table`의 `member_id column`, ...
