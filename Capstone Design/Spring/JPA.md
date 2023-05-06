# JPA





## 1. Hibernate vs Spring Data JPA

> [[JPA] JPA와 Hibernate 그리고 Spring Data JPA](https://dev-coco.tistory.com/74)
>
> [What Is the Difference Between Hibernate and Spring Data JPA?](https://dzone.com/articles/what-is-the-difference-between-hibernate-and-sprin-1)
>
> [[What's the difference between Hibernate and Spring Data JPA : stackoverflow](https://stackoverflow.com/questions/23862994/whats-the-difference-between-hibernate-and-spring-data-jpa)]

위 링크들을 토대로 요약하면, 

* Java Persistence API(JPA): API 표준 명세서(specification). Java object와 Relational database 간의 매핑
* `Hibernate` is a JPA implementation, while `Spring Data JPA` is a JPA data access abstraction. Spring Data JPA cannot work without a JPA provider.
* Therefore, Hibernate and Spring Data are `complementary` rather than competitors.





## 2. ddl-auto 옵션 관련 주의 사항

> [ddl-auto 옵션 관련 주의할 점](https://smpark1020.tistory.com/140)
>
> [[JPA] hibernate.ddl-auto 설정](https://giron.tistory.com/128)



아래 application.yml 파일을 사용하여 Spring Boot 프로젝트를 돌리는 중, Test에서 @Rollback(false) 동작 방식 관련 질문이 발생하였다. 

* [나의 질문: rollback(false) 동작방식 관련 질문](https://www.inflearn.com/questions/848582/rollback-false-%EB%8F%99%EC%9E%91%EB%B0%A9%EC%8B%9D-%EA%B4%80%EB%A0%A8-%EC%A7%88%EB%AC%B8)

```yaml
# application.yml 파일 중 일부
spring:
  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
#        show_sql: true
        format_sql: true
```



#### spring.jpa.hibernate.ddl-auto 옵션

- create: 기존테이블 삭제 후 다시 생성 (DROP + CREATE)
- create-drop: create와 같으나 종료시점에 테이블 DROP
- update: 변경분만 반영`(운영DB에서는 사용하면 안됨)`
- validate: 엔티티와 테이블이 정상 매핑되었는지만 확인
- none: 사용하지 않음(사실상 없는 값이지만 관례상 none이라고 한다.)



#### 주의사항

* 스테이징과 운영 서버는 **validate** 또는 **none** 을 사용해야 한다. 스테이징과 운영 서버에서는 테이블 생성 및 컬럼 추가, 삭제, 변경은 데이터베이스에서 직접하며, none을 사용하거나 validate를 이용해 정상적인 매핑 관계만 확인해야 한다.
* 특히, 운영 서버에는 절대 **create**, **create-drop**, **update** 를 사용하면 안된다!
* 개발 초기 단계는 **create** or **update**. 왜냐하면 처음에는 테이블의 스키마가 없기 때문이다(db에서 직접 `create table` 문으로 생성하지 않은 이상은)
* 테스트 서버는 **update** 또는 **validate**.



## 3. Entity는 직접 반환하지 않는다.

* Entity를 변경하는 순간 API 스펙이 변경될 수 있으므로, dto로 변환을 해서 controller로 반환을 하는 것을 권장한다. 





## 4. @NotNull vs @Column(nullable = false) 

> [[JPA] nullable=false와 @NotNull 비교, Hibernate Validation](https://kafcamus.tistory.com/15)

* @NotNull은 Spring Bean Validation이고, @Column(nullable = false)는 DDL 생성 시 not null이라는 조건이 붙는 것이다.
* 자세한 설명은 위 링크를 참고하자.
