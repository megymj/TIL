# Jwt 토큰 만료 응답 테스트 코드 검증



## UserControllerTest.java

```java
@Test
@DisplayName("Jwt 만료 토큰 응답 테스트")
public void jwtExpiredTest() throws Exception {
    UserRequestDto jwtExpiredDto = UserRequestDto.builder()
            .data(expiredUserJwtData)	// 만료 시간이 지난 jwt 정보가 담겨 있다. 
            .nickname(null).role(UserRole.MENTEE)
            .birthYear(1999).birthMonth(3).school("서울과학기술대학교")
            .score(3).scoreRange("후반").graduateDate(2020)
            .major("컴퓨터공학과").subMajor(null).minor(null)
            .field("프론트엔드").techStack("React, Next.js, Redux")
            .career(null).certificate(null).awards(null).activity(null)
            .build();

   // jwt 토큰 만료 시 응답 데이터 출력 테스트
    mvc.perform(MockMvcRequestBuilders.post("/users/signup")
                    .contentType(MediaType.APPLICATION_JSON)
                    .content(toJsonString(jwtExpiredDto)))
            .andExpect(status().isBadRequest())
            .andExpect(jsonPath("$.message", is("1. JWT 토큰이 만료되었습니다. 처음부터 다시 진행해 주세요 ")))
            .andDo(print());
}
```

* 현재 expiredUserJwtData에는 만료 시간이 지난 jwt의 정보가 담겨 있으며, 그 외 dto의 파라미터들은 정상적으로 매핑되어 있다. 
* 테스트를 수행하면 아래와 같은 결과가 발생한다.

<img width="1021" alt="e1" src="https://github.com/Moojun/TIL/assets/80478750/abf5d758-f7a8-4748-9562-cb4ce77aedfa">

* UserRequestDto 객체에서 Bean Validation을 사용해서 jwt 만료를 검증하고 있는데, 검증할 때 `JwtUtils class`의 decodeJwt 메소드에서 `.setSigningKey` 의 값이 null 값이 들어가서 해당 오류가 발생하는 것으로 보인다.

```java
public static Map<String, Object> decodeJwt(String jwt) {
    Jws<Claims> jws = Jwts.parserBuilder()
            .setSigningKey(getJwtSecretKey())	// 이 부분이 오류. 값이 null이 들어간다. 
            .build().parseClaimsJws(jwt);
    return jws.getBody();
}


// 메소드 내에 secretKey를 직접 선언한 뒤 대입하니, 정상적으로 테스트를 통과하는데. 
// 이렇듯 Secret Key의 정보를 코드 상에 보여주면 안된다!
public static Map<String, Object> decodeJwt(String jwt) {
    String secretKey = "[Secret Key in application-secrets.yml]"
    Jws<Claims> jws = Jwts.parserBuilder()
            .setSigningKey(jwtSecretKeyProvider(secretKey))
            .build().parseClaimsJws(jwt);
    return jws.getBody();
}
```



<br>

### 현재 Secret Key의 경우 서버에서만 가지고 있으므로, 프론트에서 회원가입 요청이 왔을 때, jwt 만료 검증 및 오류 메시지를 보내주는 기능을 구현해야함



<br>

## 해결방안

### 1. JwtUtils를 Bean으로 등록하자: 이 방법이 정확한 해결 방법인지는 아직 잘 모르겠음. 추측임

1. @Import(JwtUtils.class)
   * 실패
2. static class 정의
   * 실패

```java
@TestConfiguration
static class JwtUtilsConfig {

    @Bean
    public JwtUtils jwtUtils() {
        System.out.println("here");
        return new JwtUtils(JWT_SECRET_KEY);
    }
}
```



### 2. 정확한 오류 원인은, @Value로 주입된 SECRET_KEY를 Junit5에서는 @Value 주입 전에 코드가 실행되어서 null 오류가 발생한다. 

##### SECRET_KEY의 값이 ${jwt.secret}으로 출력되는 것의 의미는, 정상적으로 jwt.secret 정보가 등록되지 않았음을 의미한다.

<img width="733" alt="secret1" src="https://github.com/wbluke/practical-testing/assets/80478750/17ad9255-5386-4b92-97ab-90163f117192">

1. class 상단에 아래 코드 입력
   * 실패

```java
@PropertySource("classpath:application-security.yml")
@TestPropertySource("classpath:application-security.yml")
```

2. application-jwt.properties 파일을 생성 후, properties 파일을 직접 읽는 코드를 작성
   * 성공
   * 커밋 로그: 0010b43d02e7213069c4d02c

```java
public static Map<String, Object> decodeJwt(String jwt) {
        String jwtSecretKey = "";

        if (getJwtSecretKey() == null) {	// 값이 null인 경우 직접 읽는다. 
            Properties properties = new Properties();
            InputStream inputStream = JwtUtils.class.getClassLoader().getResourceAsStream("application-jwt.properties");

            try {
                if (inputStream != null) {
                    properties.load(inputStream);
                    jwtSecretKey = properties.getProperty("jwt.secret");
                    byte[] keyBytes = Decoders.BASE64.decode(jwtSecretKey);
                    JWT_SECRET_KEY = Keys.hmacShaKeyFor(keyBytes);
                }
            } catch (IOException e) {
                log.error("e = ", e);
            }
        }
        Jws<Claims> jws = Jwts.parserBuilder()
                .setSigningKey(getJwtSecretKey())
                .build().parseClaimsJws(jwt);
        return jws.getBody();
    }
```
