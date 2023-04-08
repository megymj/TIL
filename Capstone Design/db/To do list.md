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

