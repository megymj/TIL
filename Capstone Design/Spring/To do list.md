# To do list



### 1. Optional 사용

* Java8 부터 사용된 기본 문법. 요즘에는 Null을 직접 반환하는 대신 값이 없으면 `Optional`을 사용하는 추세다. 

```java
// 코드 1. 일치하는 값이 없으면 Null 반환
public Member findByLoginId(String loginId) {
    List<Member> all = findAll();
    for (Member m : all) {
        if (m.getLoginId().equals(loginId)) {
            return m;
        }
    }
    return null;
}

// 코드 2. Optional<> 사용
public Optional<Member> findByLoginId(String loginId) {
    List<Member> all = findAll();
    for (Member m : all) {
        if (m.getLoginId().equals(loginId)) {
            return Optional.of(m);
        }
    }
    return Optional.empty();
}

// 코드 3. lambda 형식 사용. 코드가 간결해짐. 추천하는 방식
public Optional<Member> findByLoginId(String loginId) {
    return findAll().stream()
            .filter(m -> m.getLoginId().equals(loginId))
            .findFirst();
}
```





### 2. logback mvc

* 실무에서 HTTP 요청시 같은 요청의 로그에 모두 같은 식별자를 자동으로 남기는 방법







### 3. ConcurrentHashMap

> [hashmap vs concurrenthashmap](https://javaconceptoftheday.com/hashmap-vs-concurrenthashmap-in-java/)
>
> [인프런 질문](https://www.inflearn.com/questions/243836/질문드립니다)
>
> [클래스 변수, 인스턴스 변수, 지역 변수 설명](https://itmining.tistory.com/20)

* 동시성이 문제가 되는 상황에서는 HashMap보다는 ConcurrentHashMap을 사용해야 한다. 예를 들어서 스프링의 빈이 싱글톤인데, 여기에서 멤버 변수를 사용하면 여러 쓰레드에서 해당 멤버 변수를 공유해서 사용하게 되고, 이때 문제가 발생할 수 있음. 
* 지역 변수는 쓰레드간에 공유되지 않기 때문에 HashMap을 사용하는 것이 올바르다.
  * 멤버 변수: 클래스 변수, 인스턴스 변수가 여기에 포함됨
  * 지역 변수
  * 두 변수에 대한 자세한 설명은 위의 링크 참고







### 4. Spring Boot Package Structure

> [Spring Boot 2.0 — Project Structure and Best Practices (Part 2)](https://medium.com/the-resonant-web/spring-boot-2-0-project-structure-and-best-practices-part-2-7137bdcba7d3)
>
> [인프런-"프로젝트 폴더 구조와 강의 일정에 관하여 질문"](https://www.inflearn.com/questions/16046/프로젝트-폴더-구조와-강의-일정에-관하여-질문이-있습니다)
>
> [Spring Guide - 패키지 구조 가이드](https://cheese10yun.github.io/spring-guide-directory/)



#### Spring Boot 서비스의 큰 흐름

<img width="813" alt="tool" src="https://user-images.githubusercontent.com/80478750/230713205-4e1e704f-94b0-4a8d-93a0-1e1e56ead283.png">

* DTO(Data Transfer Object): 데이터 전송 객체. 기능은 없고 데이터를 전달만 하는 용도로 사용되는 객체를 의미한다.
  * DTO에 기능이 있어도 상관없다. 객체의 주 목적이 데이터 전달이라면 DTO라고 할 수 있다. 

* DTO의 경우 맨 마지막에 사용되는 패키지에 넣거나, 혹은 Service, Repository 등 여러 곳에서 사용된다면 따로 dto package를 만들어서 사용하는 것도 괜찮다.





#### 계층형 구조 예시

<img width="705" alt="img1" src="https://user-images.githubusercontent.com/80478750/230712284-55765a17-645a-44da-a6c5-990c80c06d75.png">



#### 도메인형 구조 예시

<img width="712" alt="domain1" src="https://user-images.githubusercontent.com/80478750/230712285-d322a906-8553-489a-9074-b5d51b3940db.png">



#### 결론

* 패키지 구조에 정답은 없다. 프로젝트가 성장함에 따라 프로젝트 구조도 현재 상황에 맞추어 성장하고 변경된다. 
* [Spring Guide - 패키지 구조 가이드](https://cheese10yun.github.io/spring-guide-directory/) 링크를 참고하면, 최근 기술 동향에서는 도메인형 구조가 더 적합하다고 한다. 
* 단순히 개인 혹인 팀 프로젝트에서는 계층형 구조가 더 나아 보일 수 있지만, 예를 들어 controller package 내에 controller class가 여러 개가 생성되는 경우, 도메인형 구조로 변경하는 것이 나을 것으로 보인다. 



#### 기타

* `hexagonal architecture` 라는 개념이 존재하는데, 이는 추후에 알아볼 것
