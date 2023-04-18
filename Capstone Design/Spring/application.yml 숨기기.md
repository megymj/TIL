# application.yml 숨기기

Spring Boot 프로젝트를 GitHub에 올릴 때, *.properties 혹은 *.yml 파일에 있는 정보. 예를 들면, database의 url, user, password 정보 등을 올리면 보안상에 문제가 발생한다. 따라서 위의 파일은 `.gitignore`에 등록하여 숨기고 GitHub에 업로드하면 안된다.

#### 문제

* 하지만 이렇게 로컬에서 *.yml 파일을 가지고 있는 경우, 다른 사람과의 협업 시에 서로 *.yml 파일에 내용을 추가하다 보면 GitHub에서 코드를 merge 하는 과정 등에서 오류가 발생할 수 있으므로, 매번 서로 *.yml 파일을 변경할 때 마다 알려줘야 하는 번거로움이 발생할 것으로 생각된다.





### 해결 방안

#### 1. application.yml 파일 세분화

> [Hide credentials in spring boot](https://dev.to/amitiwary999/hide-credentials-in-spring-boot-1oc0)

* application.yml 파일을 GitHub에 업로드하고, env.properties 파일을 .gitignore에 등록한다. 하지만 이 방식은 크게 의미가 없어 보인다. 

```java
// application.yml 파일 예시
spring.config.import = env.properties
spring.datasource.username = DB_USER
  
// env.properties
DB_USER=name_of_sql_db_user
DB_DATABASE_NAME=name_of_database
DB_PASSWORD=database_password
GOOGLE_API_KEY=google_api_credential
```





#### 2. GitHub Secrets에 등록한 뒤, GitHub Action을 사용한 build

> [[TFAE] 스프링부트 프로젝트 GitHub Action CI&CD연대기 - 2](https://supreme-ys.tistory.com/m/161?category=951565)

추후 도전 예정

