# Spring Security and OAuth 



## 1. Spring Security



### 1-1. CSRF란

> [Spring Security CSRF 설정](https://cheese10yun.github.io/spring-csrf/)
>
> [Spring security - csrf란?](https://velog.io/@woohobi/Spring-security-csrf%EB%9E%80)
>
> [Spring Security - CSRF(Cross-Site Request Forgery)](https://zzang9ha.tistory.com/341)
>
> [Should I use CSRF protection on Rest API endpoints?](https://security.stackexchange.com/questions/166724/should-i-use-csrf-protection-on-rest-api-endpoints)

* CSRF(Cross site request forgery)란 웹 사이트의 취약점을 이용하여 이용자가 의도하지 하지 않은 요청을 통한 공격을 의미합니다. http 통신의 Stateless 특성을 이용하여 쿠키 정보만 이용해서 사용자가 의도하지 않은 다양한 공격들을 시도할 수 있습니다. 해당 웹 사이트에 로그인한 상태로 https://xxxx.com/logout URL을 호출하게 유도하면 실제 사용자는 의도하지 않은 로그아웃을 요청하게 됩니다. 실제로 로그아웃뿐만 아니라 다른 웹 호출도 가능하게 되기 때문에 보안상 위험합니다.





## 2. Google OAuth

### 구글 OAuth 로그인 구현은 크게 두 가지 방식으로 나눠지는 것으로 보인다.

### 2-1. REST API 방식

> [OAuth 2.0 동작 방식 설명](https://velog.io/@piecemaker/OAuth2-인증-방식에-대해-알아보자)
>
> [][Google Login API] [소셜 로그인 요청 Redirect 처리 - 2 (Spring Boot 레퍼런스를 보면서 구현해보는 구글 소셜 로그인 REST API - 4)](https://antdev.tistory.com/71)
>
> [Spring Boot에서 구글 소셜 로그인 REST 방식으로 구현하기](https://mslilsunshine.tistory.com/171)
>
> [Spring- 구글 OAuth 구현하기 + jWT](https://bibi6666667.tistory.com/313)
>
> [[OAuth 2.0] 스프링부트로 Google 로그인 구현해보기 -2](https://darrenlog.tistory.com/40)
>
> [Google은 Refresh Token을 쉽게 내주지 않는다.](https://hyeonic.github.io/woowacourse/dallog/google-refresh-token.html#google%E1%84%8B%E1%85%B3%E1%86%AB-refresh-token%E1%84%8B%E1%85%B3%E1%86%AF-%E1%84%89%E1%85%B1%E1%86%B8%E1%84%80%E1%85%A6-%E1%84%82%E1%85%A2%E1%84%8C%E1%85%AE%E1%84%8C%E1%85%B5-%E1%84%8B%E1%85%A1%E1%86%AD%E1%84%82%E1%85%B3%E1%86%AB%E1%84%83%E1%85%A1)

<img width="854" alt="img100" src="https://user-images.githubusercontent.com/80478750/235361059-64dcf889-7d83-4e31-b131-a0cfffdc97d1.png">

* 그림은 첫 번째 링크를 참조했으며, 절차는 그림과 같다. 
* 이 경우 구글 Redirection URI를 http://localhost:8080/auth/google/callback 형식으로 입력하여 client-id, client-secret 값을 받아야 한다. 
* https://developers.google.com/youtube/v3/guides/auth/server-side-web-apps?hl=ko 구글 공식 문서를 참고해서 `client_id`, `redirect_uri`, `response_type`, `scope` 등의 정보를 Google의 OAuth 2.0 엔드포인트인`https://accounts.google.com/o/oauth2/v2/auth` 로 전달하여 통신하는 방식
*  `code` 값을 받은 뒤 구글 서버로 `access token` 요청을 보내 받아온 뒤, access token을 제공해서 유저 정보를 받아온다. 



### 2-2. 인증된 OAuth2User 객체를 반환받아 처리하는 방식

> [이동욱님](https://jojoldu.tistory.com/) 저서 '[스프링 부트와 AWS로 혼자 구현하는 웹 서비스](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788965402602)'
>
> [Spring Boot 게시판 OAuth 2.0 구글 로그인 구현](https://dev-coco.tistory.com/128#head6)
>
> [Springboot 구글 OAuth2 로그인 + JWT](https://velog.io/@rudwhd515/Springboot-JWT)
>
> [구글, 카카오, 네이버, 페이스북 로그인 기본 절차](https://developerbee.tistory.com/245), [[Spring Boot] OAuth2 소셜 로그인 가이드 (구글, 페이스북, 네이버, 카카오)](https://deeplify.dev/back-end/spring/oauth2-social-login)

* `OAuth2UserService<OAuth2UserRequest, OAuth2User>` or `DefaultOAuth2UserService` 인터페이스를 상속받아 `loadUser` 메소드를 Override해서 `userRequest` 정보를 받아 오는 방식



#### 이 중 REST API를 구현하는 방식으로 공식 문서를 참고하여 코드를 작성하였음.

* 참고한 공식 문서: https://developers.google.com/youtube/v3/guides/auth/server-side-web-apps?hl=ko 

* 아래 사진 참고: https://developers.google.com/identity/protocols/oauth2

<img width="589" alt="imga3" src="https://user-images.githubusercontent.com/80478750/235443917-c4e3639a-dd47-4b1d-8611-08c58e3cf9ff.png">







## 3. KaKao OAuth

### KaKao OAuth 로그인 구현 방식 역시 두 가지 방식으로 나눠진다.

### 3-1. Rest API 방식

> [[Spring Boot] 카카오 OAuth2.0 구현하기](https://velog.io/@leesomyoung/Spring-Boot-%EC%B9%B4%EC%B9%B4%EC%98%A4-OAuth2.0-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)
>
> [[Kakao Login API] 카카오 계정의 유저 정보 받아오기 및 마무리 (Spring Boot 환경에서 카카오 로그인 API RESTful방식으로 연동하기 -4장 마무리) (0)](https://antdev.tistory.com/37)





### 3-2. 인증된 OAuth2User 객체를 반환받아 처리하는 방식

> [구글, 카카오, 네이버, 페이스북 로그인 기본 절차](https://developerbee.tistory.com/245), [[Spring Boot] OAuth2 소셜 로그인 가이드 (구글, 페이스북, 네이버, 카카오)](https://deeplify.dev/back-end/spring/oauth2-social-login)



### 구글 로그인 구현과 마찬가지로 REST API를 구현하는 방식으로 공식 문서를 참고하여 코드를 작성하였음.

* [REST API | Kakao Developers](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#req-user-info) 참고
* 카카오 Auth Server에 Access token을 요청한 다음 카카오 유저 데이터를 받아오는 과정(https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api#req-user-info)에서, ObjectMapper 를 사용해서 JSON 데이터를 Java dto 객체로 변환하였는데, 이 때 내부 클래스를 중첩해서 사용해야 하는 상황이 발생해서 코드가 지저분해졌다.
  * [[Java] ObjectMapper를 이용하여 JSON 파싱하기 ](https://velog.io/@zooneon/Java-ObjectMapper를-이용하여-JSON-파싱하기#내가-시도했던-방법-세-번째) 참고

```java
@Getter
@Builder
@NoArgsConstructor
@AllArgsConstructor
@JsonIgnoreProperties(ignoreUnknown = true)
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)   
public class KaKaoOAuthUser {

    private String id;  
    private KakaoAccount kakaoAccount;

    @Getter
    @NoArgsConstructor
    @JsonIgnoreProperties(ignoreUnknown = true)
    @JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
    public static class KakaoAccount {
        private Profile profile;
        private String email;

        @Getter
        @NoArgsConstructor
        @JsonIgnoreProperties(ignoreUnknown = true)
        public static class Profile {
            private String nickname;
        }
    }
}
```



#### P.S] 현재 프론트 팀은 Next.js를 사용하는데, NextAuth.js 라이브러리를 사용하면 Google, Facebook, Kakao, LINE, Instagram 등 소셜 로그인 기능을 쉽게 구현할 수 있다...

* https://next-auth.js.org/getting-started/client
* 그래도 일단 서버에서 Google 로그인 절차를 개발했으므로 서버에서 소셜 로그인 처리를 진행하는 것으로 진행할 계획.



## 4. 서비스 회원가입 이후 사용자에게 추가 정보 입력받기

<img width="595" alt="google login" src="https://user-images.githubusercontent.com/80478750/235340267-241d08e8-a6c2-4cfd-95c1-3dc85b4faddd.png">

* 구글의 많은 레퍼런스에는 단순히 구글 로그인에 대한 부분만 고려하였으나, 본 서비스에는 구글 로그인 이후, 추가적인 정보(학교, 졸업 예정 연도, 학점 등의 필수 정보와 자격증, 경력 등의 선택 정보)를 사용자에게 입력받아야 한다.
* 위 사진은 **알바몬** 회원가입 예시로, 회원가입 시 구글을 선택한 이후 추가적인 정보 입력을 요청한다.
* 현재 생각 중인 방식은 두 가지이다.

  1. Google Server로 access token 정보를 요청하여 Google User Data를 가져온 다음, 사용자에게 추가 정보 입력을 받은 뒤 한 번에 db로 insert한다.

  2. 먼저 구글 로그인 이후 구글에 가입된 유저의 정보를 table에 먼저 insert 한 뒤, 사용자가 추가 정보를 입력하면 해당 정보들을 기존 데이터에 update한다. 

     * 이 경우 사용자의 e-mail, provider(google or kakao) 등 여러 정보를 가지고 있어야 한다.

     * 사용자가 입력하는 추가 정보 중 필수 정보와 선택 정보가 있는데, 필수 정보의 경우 프론트에서 사용자가 입력하지 않았을 경우 다음 화면으로 넘어가지 못하게 할 수 있다. 
* 하지만 2번의 경우는 적절하지 않다고 판단하였다. 그 이유는,
  1. User table을 설계할 당시, 추가적인 정보 중 필수 정보는 column을 not null로 설정할 것이므로, 유저의 정보를 먼저 table에 insert 할 때 not null column에는 dummy data를 삽입해야 한다.
  2. 만약 사용자가 구글 로그인 이후, 추가 정보를 입력하는 도중 브라우저를 닫은 뒤 다시 열면, 다음 번에 사용자가 다시 구글 로그인을 통해 회원 가입 시 처리하는 과정이 꼬인다.



#### 따라서 1번 방식, 즉 Google Server로 access token 정보를 요청하여 Google User Data를 가져온 다음, 사용자에게 추가 정보 입력을 받은 뒤 한 번에 db로 insert한다.

이 방식에도, 두 가지 방법이 존재한다.

1. Google User Data를 받은 뒤, 서버에서 in-memory db에 저장하였다가, 클라이언트로부터 추가 정보 데이터를 받은 다음 in-memory db를 조회하여 데이터를 꺼내온 뒤, 한 번에 db에 insert한다.
2. Google User Data를 JWT 등을 사용하여 암호화한 다음, 클라이언트에 전달한다. 클라이언트에서는 복호화 과정을 통해 데이터를 검증한 다음, 추가 정보와 함께 서버로 전달한다. 이때, Google User Data는 주요 정보이므로 header에 포함하고, 추가 정보는 Body에 담아서 보내는 등의 처리를 수행한다.

1번 방식보다는 2번 방식이 기능 단위로 보았을 때 더 효과적인 것으로 생각되어서, 2번 방식으로 결정함.





## 5. JWT. Access Token, Refresh Token을 사용한 인증

> [JWT. Access Token, Refresh Token 설명](https://ksh-coding.tistory.com/58)

* https://jwt.io JSON Web Token 공식 사이트에 들어가면 JWT를 구현한 Java open soruce가 약 10개 정도 있다. 이 중 [java-jwt](https://github.com/auth0/java-jwt) 와 [jjwt](https://github.com/jwtk/jjwt) 오픈소스가 많이 사용되는 것으로 보이며, 이 중 README.md 가 더 상세한 `jjwt` 라이브러리를 사용하기로 결정하였다. 
