# Spring Annotation 정리



### @Transactional 

> [Service 계층에서 작성하는 이유](https://developer-talk.tistory.com/418)
>
> [JPA 사용시 테스트 코드에서 @Transactional 주의하기](https://javabom.tistory.com/103)
>
> [왜 Test에 transactional을 또 붙여야하나요?](https://www.inflearn.com/questions/257700/왜-test에-transactional을-또-붙여야하나요)

* 일반적으로 Transactional annotation은 Repsoitory 계층이 아닌 Service 계층에 작성한다. 그 이유는, 하나의 서비스 계층에서 여러 개의 DAO를 호출할 수 있는데, 이 때 DAO 계층에 Transactioal annotation을 작성하게 되면 일관성이 유지되지 않을 가능성이 존재한다.
* Test에서 @Transactional 을 붙이게 되면 데이터베이스에 저장된 결과를 커밋하지 않고 롤백한다. 그래서 테스트를 여러번 실행해도 동일한 상황에서 테스트를 할 수 있다. 
  * 즉, Create, Update, Delete 등의 로직이 정상적으로 수행됨을 확인하면서도 실제 db에 반영되지 않는다. 
  * 데이터가 추가, 변경, 삭제 되는 과정을 확인하고 싶다면, `@Rollback(false)` 을 붙여주면 된다. 
