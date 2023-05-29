# 테스트 코드 원칙 정리

> reference link: [Junit5 User Guide Documents](https://junit.org/junit5/docs/current/user-guide/)





## 1. Test class, test methods, and lifecycle methods are not required to be `public`, but they must *not* be `private`.

* IntelliJ에서 test class를 만들면 아래와 같은 양식으로 만들어진다. 하지만 Junit5 부터는 class 명칭 앞에 `public` 을 생략해도 상관없다. 
* Junit5 Document 2.3에서, 기술적인 문제가 없다면 test class, test methods, and lifecycle methods에 `public` 을 붙이는 것을 권장하지 않는다고 작성되어 있다. 

```java
class SessionManagerTest {
  
}
```





## 2. JpaRepository에서 테스트 코드를 어느 정도까지 작성해야 하는가?

1. JpaRepository 를 구현하는 구현체로 기본적인 CRUD 등 (ex. save(), saveAll() 등) 사용한다면, 이 메소드들에 대해서도 테스트 코드를 작성해야 하는가?
   * 기본으로 제공되는 메서드는 테스트 코드를 굳이 작성할 필요가 없다. 제공되는 기능은 이미 검증 자체가 완료된, 라이브러리로 제공되는 기능이기 때문에 굳이 우리가 비용을 들여 테스트할 필요는 없다.
   * 하지만 findAllById 등의 Jpa 메서드, Jpql, queryDsl 등의 것들은 테스트 코드를 작성하는 것이 좋다. 왜냐하면 내가 생각한 의도대로 동작하지 않을 수 있기 때문이다. 





## 3. MockMvc

- 스프링에서 MVC 테스트를 하기위한 방법을 논의하여 Spring-test모듈을 스프링 프레임워크에 더한 것
- 브라우저에서 요청과 응답을 의마하는 객체로서 Controller 테스트를 용이하게 해주는 라이브러리
- 기존의 MockHttpServletRequest, MockHttpServletREsponse를 활용한 단위테스트에서 발전되었음