# Intellij] Cannot resolve method 'setAttribute' error

JSP, Servlet을 사용한 웹 프로젝트 도중, 아래처럼 "Cannot resolve method setAttribute" 창이 발생함.

하지만, 아래 사진처럼 빨간 글씨로 오류가 발생하는 것과는 다르게, 정상적으로 실행은 잘 되었다. 

<img width="775" alt="error" src="https://user-images.githubusercontent.com/80478750/202837111-d2b85933-459c-4e0f-b8a1-42960badf15d.png">



* 실행에 큰 문제는 없지만 빨간 글씨가 보기에 불편해서, 해당 오류를 제거하고자 하였다.

* 검색한 결과, 위 오류는 tomcat > lib 디렉토리 안에 있는 servlet-api.jar, jsp-api.jar를 import 시켜줘야 한다고 한다.





### 오류 해결

> IntelliJ 내에서 Project setting에서 Libraries에 위의 두 .jar 파일을 추가했으나, 여전히 오류가 발생했다. 
>
> 그래서, 그냥 build.gradle 내부에 dependencies를 추가하고 sync하니 잘 해결이 되었다. 

```java
// https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api
compileOnly group: 'javax.servlet', name: 'javax.servlet-api', version: '3.1.0'
// https://mvnrepository.com/artifact/javax.servlet/jsp-api
compileOnly group: 'javax.servlet', name: 'jsp-api', version: '2.0'
```



<img width="731" alt="solve" src="https://user-images.githubusercontent.com/80478750/202837439-82c14e5a-59af-404c-a3ca-c5290587315c.png">

