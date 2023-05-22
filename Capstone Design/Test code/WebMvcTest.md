# @WebMvcTest





## 1. UserControllerTest

```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = UserController.class)
class UserControllerTest {

    @Autowired
    private MockMvc mvc;

    @MockBean
    private UserService userService;

    @Test
    @DisplayName("닉네임 검증; 공백 체크")
    public void testNicknameDuplicateBlank() throws Exception {
        mvc.perform(get("/users/check-nickname")
                        .queryParam("nickname", "  "))
                .andExpect(status().isBadRequest())
                .andDo(print());
    }
}
```

1. @MockBean을 사용해서 UserService를 주입하지 않으면 오류 발생한다.

   >  [[SpringBoot] @WebMvcTest 단위 테스트시 Bean 주입 에러](https://velog.io/@gillog/SpringBoot-WebMvcTest-%EB%8B%A8%EC%9C%84-%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%8B%9C-Bean-%EC%A3%BC%EC%9E%85-%EC%97%90%EB%9F%AC)

2. `.andDo(print())` 를 통해 요청/응답 메시지 전체를 확인할 수 있다.



### 내가 기대한 결과

* `.andExpect(status().isBadRequest())` 의 결과는 400이므로, 테스트가 성공적으로 수행되어야 한다.

```json
{
    "status": 400,
    "errorName": "BAD_REQUEST",
    "message": "Nickname only contains blank",
    "code": "U002"
}
```



### 실제 결과

* 401 코드가 출력되어, 400과 다르므로 테스트가 실패한다.

```java
MockHttpServletResponse:
           Status = 401
    Error message = Unauthorized
          Headers = [WWW-Authenticate:"Basic realm="Realm"", X-Content-Type-Options:"nosniff", X-XSS-Protection:"1; mode=block", Cache-Control:"no-cache, no-store, max-age=0, must-revalidate", Pragma:"no-cache", Expires:"0", X-Frame-Options:"DENY"]
     Content type = null
             Body = 
    Forwarded URL = null
   Redirected URL = null
          Cookies = []
```



### 오류 해결

>  [Spring Security REST - Unit Tests fail with HttpStatusCode 401 Unauthorized](https://stackoverflow.com/questions/50209302/spring-security-rest-unit-tests-fail-with-httpstatuscode-401-unauthorized)

```java
@ExtendWith(SpringExtension.class)
@WebMvcTest(controllers = UserController.class, excludeAutoConfiguration = {SecurityAutoConfiguration.class})
class UserControllerTest {

    @Autowired
    private MockMvc mvc;

    @MockBean
    private UserService userService;

    @Test
    @DisplayName("닉네임 검증; 공백 체크")
    public void testNicknameDuplicateBlank() throws Exception {
        mvc.perform(get("/users/check-nickname")
                        .queryParam("nickname", "  "))
                .andExpect(status().isBadRequest())
                .andDo(print());
    }
}
```

* @WebMvcTest 어노테이션에 exclude ... 부분을 추가하니 테스트가 성공함
  * 하지만 이 부분을 추가해야하는 원인은 아직 모르겠다.
  * 5/22 해당 내용과 유사한 내용의 글을 발견함. [**스프링부트 JUnit5 - Error creating bean with name 'securityConfig' defined in file 해결법**](https://pgmjun.tistory.com/75)





## 2. @WebMvcTest는 Service나 Repository class를 주입 받을 수 없다. 

> [테스트할 때 MockMvc 주입받는 방법](https://smpark1020.tistory.com/163)

따라서 Controller만 테스트할 경우에 사용한다. 이 때, @WebMvcTest를 사용하면 MockMvc를 주입받아서 사용할 수 있다.

