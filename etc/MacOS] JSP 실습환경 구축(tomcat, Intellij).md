# MacOS] JSP 실습환경 구축(tomcat, Intellij)



<br>



## 1. tomcat 설치
> sites: [link](tomcat.apache.org)

'Tomcat 9' 선택(Tomcat 9의 경우 Java8 부터 지원한다고 한다)

MacOS이므로, tar.gz 설치
![](https://velog.velcdn.com/images/moojun3/post/c93463f7-01c4-4952-9199-6182d2091d86/image.png)

tar.gz 압축파일을 풀어주면, tomcat 설치가 완료됨.
압축해제된 폴더는 원하는 위치로 옮긴다.

<br>

## 1-1. tomcat 실행 
tomcat이 설치된 경로/bin 으로 이동한다.

```bash
ex] cd Desktop/apache-tomcat-9.0.64/bin

# server start
./startup.sh

# server stop
./shutdown.sh

```

아래 사진은 서버를 시작한 뒤, localhost:8080 으로 접속 시 나타나는 페이지이다.
![](https://velog.velcdn.com/images/moojun3/post/89a14f51-d0f0-41f7-a47c-d13438142205/image.png)

<br>

## 2. Intellij 설정
> reference link: [Tomcat in IntelliJ : youtube](https://www.youtube.com/watch?v=ThBw3WBTw9Q&t=794s)



#### 1. Create New Project

* Tomcat 경로는 1번에서 설치한 경로를 지정
![](https://velog.velcdn.com/images/moojun3/post/76d360cb-4f65-4866-9bb5-335f360f3b47/image.png)

#### 2. localhost path 수정

* Run > Edit Configurations... 클릭
  ![](https://velog.velcdn.com/images/moojun3/post/4c8e7917-8ecf-4ca0-8d77-ebe56191bb99/image.png)

  

* Application context에 경로를 '/' 혹은 원하는 경로로 지정
![](https://velog.velcdn.com/images/moojun3/post/31663af6-ef3b-4dc6-9979-ba9ef2b2b965/image.png)