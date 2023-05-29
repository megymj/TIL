# Capstone Design 



## 1. OAuth(Google, Kakao)를 이용한 회원가입 절차

1. 사용자가 Google, Kakao 계정을 입력한다; e-mail, password

   * 이후 Google, Kakao API 서버와 통신하여 User 정보(e-mail, name, id) 등을 받아온다.

   * 요청으로 들어오는 e-mail, name, provider를 확인하여, 만약 기존에 가입된 사용자라면(db에 User 정보가 존재한다면) Exception 처리
2. Google, Kakao 인증 이후 User 정보를 jwt로 암호화해서 클라이언트로 보낸다.
   * JWT 유효 기간은 2시간으로 설정
   * expired time 이후에는 jwt를 사용할 수 없다.
3. 사용자가 클라이언트에서 추가 정보를 입력한 뒤 서버로 POST 요청을 보낸다.

   * 추가 정보 입력 과정에서, 닉네임 중복 검사를 진행한다.
     * 닉네임은 unique 값으로, 이미 닉네임이 db에 존재하면=User 정보가 이미 존재하면, CustomException 처리
   * 사용자는 필수 정보와 선택 정보를 입력하는데, 필수 정보의 경우 DTO 객체를 만들어서 @NonBlank 등의 Annotation으로 Bean Validation 절차를 거친다.
     * @Valid, @RequestParam, BindingResult 사용
     * jwt 토큰의 경우, `Custom valid Annotation` 을 생성해서 만료 여부 검증: `@Expired` 
     * BindingResult 검증 결과 오류 발생 시 CustomException 처리
   * 데이터가 정상적으로 잘 들어 오면, db에 저장 후 Created 201 응답코드 전달



## 1-1. CORS 이슈 해결

* Filter와 WebMvcConfig에서 관련 코드들을 추가해서 해결.
  * 다른 블로그를 참고하니, 보통 WebMvcConfig만 CORS를 설정하면 되던데, 나의 경우 Filter를 제외하면 UserControllerTest에서 오류가 발생해서 두 부분을 모두 추가함.
  * 사실상 `CustomCorsFilter` 에서 CORS를 해결해주는 것으로 보이며, 이 부분을 주석처리한 뒤 테스트하면 CORS 이슈가 발생한다. 
  
  <img width="797" alt="img1" src="https://github.com/Moojun/TIL/assets/80478750/4de32b6b-7509-4439-a29d-399d302b9e4b">



## 2. 로그인 절차 수행

1. 로그인도 마찬가지로 Google, Kakao Redirect URI를 통해 Google, Kakao API 서버의 인증을 거쳐야 한다. 1번과 마찬가지 url(`auth/google`, `auth/kakao`) 를 사용한다. 
2. 로그인 요청 시 소셜 로그인 유저의 정보가 db에도 저장되어 있는지 확인 후, Access Token 및 Refresh Token을 발급한다. Token의 경우 JWT를 사용한다. 
