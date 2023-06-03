# Capstone Design 



## 0. CI/CD
> [배포 2: GitHub Action, Docker, EC2](https://github.com/Moojun/TIL/blob/main/Capstone%20Design/AWS/%EB%B0%B0%ED%8F%AC%202%5D%20Github%20Action%2C%20Docker%2C%20EC2.md)





## 1. AWS

### 1-1. EC2

1. AMI: Ubuntu 20.04

2. Timezone 변경: UTC -> KST
   * [참고링크](https://github.com/Moojun/TIL/blob/main/AWS/Develop-Environment/EC2%EC%97%90%20JSP%2C%20Servlet%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EB%B0%B0%ED%8F%AC.md) 1-6 참고

3. Install Docker in Ubuntu 20.04
   * https://docs.docker.com/engine/install/ubuntu/

4. Prod(main branch), Dev(develop branch) 서버를 분리해서 운영하려고 하였으나, 프로젝트 규모가 크지 않고 서버 비용이 많지 않아서, main branch를 Prod 및 Dev 서버로 운영하기로 하였음
   1. Prod: port 80(http default port)







## 2. OAuth(Google, Kakao)를 이용한 회원가입 절차

1. REST API 방식으로 개발함

   1. 사용자가 Google, Kakao 계정을 입력한다; e-mail, password

   2. 이후 Google, Kakao API 서버와 통신하여 User 정보(e-mail, name, id) 등을 받아온다.

     * 구글의 경우 아래 그림과 절차가 같으며, 카카오도 이와 유사함.

     <img width="854" alt="img100" src="https://user-images.githubusercontent.com/80478750/235361059-64dcf889-7d83-4e31-b131-a0cfffdc97d1.png">

   3. 요청으로 들어오는 e-mail, name, provider를 확인하여, 만약 기존에 가입된 사용자라면(db에 User 정보가 존재한다면) CustomException 처리

2. Google, Kakao 인증 이후 User 정보를 JWT로 토큰화(암호화)해서 클라이언트로 보낸다.
   * JWT 유효 기간은 2시간으로 설정
   * expired time 이후에는 jwt를 사용할 수 없다.

3. 사용자가 클라이언트에서 jwt data와 추가 정보를 입력한 뒤, 서버로 POST 요청(회원 가입 요청)을 보낸다.

   * 추가 정보 입력 과정에서, 닉네임 중복 검사를 진행한다.
     * 닉네임은 unique 값으로, 이미 닉네임이 db에 존재하면(User 정보가 이미 존재하면), CustomException 처리
   * 사용자는 필수 입력 정보와 선택 입력 정보를 입력한 뒤 json 형식으로 서버로 요청을 전달하면, 서버에서 해당 json 데이터를 DTO 형식으로 받는다.  필수 입력 정보의 경우 DTO 객체 내에서 @NonBlank 등의 Annotation으로 Bean Validation 절차를 거친다.
     * @Valid, @RequestBody, BindingResult 사용
     * jwt 토큰의 경우, `Custom valid Annotation` 을 생성해서 만료 여부 검증: `@Expired` 
     * BindingResult 검증 결과 오류 발생 시 CustomException 처리
   * 하지만 DTO String 값으로 `"userA"` 가 아닌 `"userA`  처럼 큰따옴표가 없는 등의 이상한 텍스트가 입력값으로 들어오면 Bean Validation이 동작하기 전에 `JsonMappingException` 이 발생한다
     * 이에 대한 내용은, [@RequestBody DTO Validation](https://github.com/Moojun/TIL/blob/main/Capstone%20Design/Spring/%40RequestBody%20DTO%20Validation.md)
   * 데이터가 정상적으로 잘 들어 오면, db에 저장 후 Created 201 응답코드 전달



## 2-1. CORS 이슈 해결

* CustomCorsFilter와 WebMvcConfig에서 관련 코드들을 추가해서 해결.
  
  * 처음에는 `CorsFilter`로 필터 명칭을 사용하려고 하였으나, Spring Security의 Filter 중 이미 `CorsFilter` 가 존재해서 중복을 방지하기 위해 명칭을 `CustomCorsFilter`로 사용.
  * 다른 블로그를 참고하니, 보통 WebMvcConfig만 CORS를 설정하면 되던데, 나의 경우 Filter를 제외하면 UserControllerTest에서 오류가 발생해서 두 부분을 모두 추가함.
  * 사실상 `CustomCorsFilter` 에서 CORS를 해결해주는 것으로 보이며, 이 부분을 주석처리한 뒤 테스트하면 CORS 이슈가 발생한다. 
  * https://github.com/Moojun/cors-test-repo 를 통해 CORS 여부를 테스트 가능하다. 
  
  <img width="797" alt="img1" src="https://github.com/Moojun/TIL/assets/80478750/4de32b6b-7509-4439-a29d-399d302b9e4b">



## 3. 로그인; JWT를 사용한 인증/인가

1. 로그인도 마찬가지로 Google, Kakao Redirect URI를 통해 Google, Kakao API 서버의 인증을 거쳐야 한다. 1번과 마찬가지 url(`auth/google`, `auth/kakao`) 를 사용한다. 
2. 로그인 요청 시 소셜 로그인 유저의 정보가 db에도 저장되어 있는지 확인 후, Access Token 및 Refresh Token을 발급한다. Token의 경우 JWT를 사용한다. 
