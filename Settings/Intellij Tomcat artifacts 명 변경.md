# Intellij Tomcat artifacts 명 변경

웹서버 프로젝트 진행 중 package 명칭을 com.seoultech.Book24 -> com.seoultech.Stock24로 변경하였으나, Intellij에서는 artifacts가 여전히 com.seoultech.Book24로 뜨는 현상이 발생하였다.





### 해결

> settings.gradle에서 rootProject.name을 변경한 뒤 sync하니, 정상적으로 처리됨
>
> 정상적으로 변경된 모습을 아래 사진에서 확인할 수 있다. 

<img width="1016" alt="Screenshot 2022-11-12 at 11 02 05 PM" src="https://user-images.githubusercontent.com/80478750/201478183-962bf608-ab2f-487f-9946-5bbbca2e9404.png">