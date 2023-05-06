# Spring Annotation 정리



## @Transactional 

> [Service 계층에서 작성하는 이유](https://developer-talk.tistory.com/418)
>
> [JPA 사용시 테스트 코드에서 @Transactional 주의하기](https://javabom.tistory.com/103)
>
> [왜 Test에 transactional을 또 붙여야하나요?](https://www.inflearn.com/questions/257700/왜-test에-transactional을-또-붙여야하나요)

* 일반적으로 Transactional annotation은 Repsoitory 계층이 아닌 Service 계층에 작성한다. 그 이유는, 하나의 서비스 계층에서 여러 개의 DAO를 호출할 수 있는데, 이 때 DAO 계층에 Transactioal annotation을 작성하게 되면 일관성이 유지되지 않을 가능성이 존재한다.
* Test에서 @Transactional 을 붙이게 되면 데이터베이스에 저장된 결과를 커밋하지 않고 롤백한다. 그래서 테스트를 여러번 실행해도 동일한 상황에서 테스트를 할 수 있다. 
  * 즉, Create, Update, Delete 등의 로직이 정상적으로 수행됨을 확인하면서도 실제 db에 반영되지 않는다. 
  * 데이터가 추가, 변경, 삭제 되는 과정을 확인하고 싶다면, `@Rollback(false)` 을 붙여주면 된다. 



## @Builder

> [[Java] 빌더 패턴(Builder Pattern)을 사용해야 하는 이유](https://mangkyu.tistory.com/163)
>
> [Builder Pattern을 써야 하는 이유](https://pamyferret.tistory.com/67)

* 생성자를 사용하는 것보다는 Lombok의 @Builder를 사용하는 것이 더 효율적이다. 



## @Bean vs @Component

> [@Bean vs @Component](https://jojoldu.tistory.com/27)

* 스프링은 개발의 제어권이 스프링 컨테이너(IOC Container)에 있다고 해서, 이를 IoC(Inversion Of Control), 제어의 역전이라고 한다. 

* 스프링이 개발자 대신 객체를 제어하기 위해서는 객체들이 Bean으로 등록이 되어 있어야 한다. 과거에는 객체를 빈으로 등록하기 위해 XML로 지정해야 했으나, 현재는 Annotation으로 간단하게 등록할 수 있다. 

* @Bean: 개발자가 컨트롤이 불가능한 **외부 라이브러리들을 Bean으로 등록하고 싶은 경우**에 사용한다.
  * 예를 들면 ObjectMapper의 경우 ObjectMapper Class에 @Component를 선언할수는 없으니 ObjectMapper의 인스턴스를 생성하는 메소드를 만들고 해당 메소드에 @Bean을 선언하여 Bean으로 등록한다.
* @Component: 개발자가 **직접 컨트롤이 가능한 Class들의 경우**에 사용한다.
