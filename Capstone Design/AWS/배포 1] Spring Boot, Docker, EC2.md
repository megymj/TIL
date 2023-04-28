# 배포 1] Spring Boot, Docker, EC2

> [참고링크1](https://bcp0109.tistory.com/356)
>
> [참고링크2](https://develop-writing.tistory.com/121)

기존에 [개인 프로젝트](https://github.com/Moojun/Stock24)를 진행할 때는, [EC2에 Tomcat Servlet 프로젝트 배포](https://github.com/Moojun/TIL/blob/main/AWS/Develop-Environment/EC2%EC%97%90%20JSP%2C%20Servlet%20%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%20%EB%B0%B0%ED%8F%AC.md) 문서처럼 GitHub에서 수정 사항이 발생할 때마다, git pull, tomcat 실행 등 여러 명령어를 매번 입력하던 번거로움이 존재하였는데,  참고 링크 2처럼, 배포 스크립트를 .sh 파일로 만들어서, 변경 사항이 발생할 때마다 여러 명령어를 한 번에 수행하거나, 아래에서 Docker Images를 생성하는 등 배포 과정을 단순화 시킨다면 훨씬 더 효율적으로 프로젝트를 진행할 수 있겠다는 생각이 들었다. 



## 1. Docker

> [도커를 쓰는 이유](https://www.44bits.io/ko/post/why-should-i-use-docker-container)
>
> [Top 5 Advatages of using Docker](https://www.cloudtern.com/cloud/top-5-advantages-of-using-docker/)
>
> [Why should I use docker with ec2? Why not ec2 alone?](https://www.reddit.com/r/aws/comments/c4cnpz/why_should_i_use_docker_with_ec2_why_not_ec2_alone/)



### 도커의 장점

1.  Consistent Environment: Dockerfile을 통해 서버 운영기록을 코드로 남길 수 있다. 또한, 서로 다른 환경에서 Dockerfile을 통해 동일하게 실행될 수 있다.-&gt; 이는 팀원이 협업할 때, 프레임워크 버전 등 여러 버전을 일치시킬 수 있어서 쉬울 것으로 보인다. 
2. 서버의 확장: 만약 프로젝트 규모가 커져서 서버를 옮기거나 확장해야 할 때, 도커를 사용하면 Docker Image만 가져와 새로운 서버에 동일한 환경을 쉽게 구축할 수 있다. 
3. 그 외 이유들은 위 링크 혹은 구글링 참고



### 개인 프로젝트에서 Docker를 사용해야 하는 이유?

현재 프로젝트에서 Docker가 사용되는 곳은 크게 두 가지임(추후 추가될 수 있음)

1. EC2에 Spring Boot 배포 
2. 문장 유사도 검증시 AWS Lambda를 사용하는데, 이 때 python code를 Docker image로 업로드

이 중 2번의 경우, 여러 머신러닝 라이브러리를 코드에서 사용하기 때문에 Docker를 사용하는 것이 거의 반강제이나, 1번의 경우 실제 배포해서 창업을 위한 프로젝트가 아닌 졸업 작품 프로젝트이므로 EC2 Instance를 하나 이상 사용할 것 같지 않다. 따라서 EC2 인스턴스에 초기 환경 구축(Java 11 설치 등)을 한 이후, 환경이 수정될 것 같지는 않지만, 원활한 관리 및 추후 서버 확장 가능성을 고려하여 Docker를 사용하기로 하였다. 



## 2. Spring Boot + Docker + EC2 배포

> [참고 1](https://velog.io/@msung99/Docker-도커를-이용한-스프링-부트-SNAPSHOT-jar-파일-배포하기)
>
> [참고 2](https://devfoxstar.github.io/java/springboot-docker-ec2-deploy/)
>
> [참고 3](https://zzang9ha.tistory.com/360)



### Docker를 사용해 EC2에 이미지가 업로드되는 절차

<img width="762" alt="img1" src="https://user-images.githubusercontent.com/80478750/232288059-8dc184b0-8e4d-4836-9a9c-6b9f46028a24.png">

1. Dockerfile을 build해서 Docker image 파일을 생성
2. Docker image 파일을 dockerhub으로 push
3. EC2에서 dockerhub에 존재하는 docker image 파일을 pull로 받아온다.
4. docker run 명령 등을 통해 docker image 파일 실행



### 2-1. Intellij .jar 파일 빌드

```bash
./gradlew clean
./gradlew build

# -x test: 테스트 실행 X
./gradlew build -x test 
```



### 2-2. Dockerfile 생성 및 dockerhub 리포지토리 생성

```dockerfile
FROM openjdk:11
ARG JAR_FILE=*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```



### build.gradle 수정

> [stackoverflow](https://stackoverflow.com/questions/67663728/spring-boot-2-5-0-generates-plain-jar-file-can-i-remove-it)



위 스택오버플로우에 따르면, Spring Boot 2.5.0 이후로 .jar 및 -plain.jar 파일 2개가 생성되어서, Docker image가 생성되지 않는 오류가 발생할 수 있으므로, 아래 내용을 추가해야 한다. 

```java
// build.gradle에 아래 내용 추가

jar {
    enabled = false
}
```



이후 dockerhub에서 repository를 생성한다. 

[내 docker repository](https://hub.docker.com/r/moojun/menzil-be/tags)



### docker tag란?

> https://www.baeldung.com/ops/docker-tag

The Docker tag helps maintain the build version to push the image to the Docker Hub**. The Docker Hub allows us to group images together based on name and tag.** Multiple Docker tags can point to a particular image. Basically, As in Git, `Docker tags are similar to a specific commit`. Docker tags are just an alias for an image ID.



### 2-3. Docker image 생성 및 dockerhub에 push

```dockerfile
# macOS M1 chip의 경우 --platform linux/amd64 옵션을 추가해야 함
docker build --platform linux/amd64 -t moojun/menzil-be:[tag] .
# e.g) docker build --platform linux/amd64 -t moojun/menzil-be:0.0.1 .

# 정상 build 조회
docker images

# push dockerhub
docker push moojun/menzil-be:[tag]
```



### 2-4. EC2 기본 환경 구축

* EC2, Security Group 생성 및 Elastic IP 설정(내용 생략)
* EC2에 Docker 설치
  * https://docs.docker.com/engine/install/ubuntu/

* docker image pull

```bash
# pull 
sudo docker pull moojun/menzil-be:[tag]

# run docker
sudo docker run -p 8080:8080 moojun/menzil-be:[tag]

# Run docker background
sudo docker run -d --name 'test1' -p 8080:8080 moojun/menzil-be:[tag]

# show log
sudo docker log [container id]
```



### 2-5. Docker 명령어

```bash
# 현재 이미지 확인
sudo docker images

# 이미지 삭제
sudo docker rmi [image id]

# 컨테이너 확인
sudo docker container ls -a

# 동작 중인 컨테이너 확인
sudo docker ps

# 컨테이너 중지
sudo docker stop [container name or id]

# 컨테이너 삭제
sudo docker rm [container id]
```



## 결론

* `gradle .jar build -> Dockerfile build -> image push -> image pull -> docker run` 과정을 통해 프로젝트를 EC2에 배포할 수 있다. 

* 하지만 이 과정에서, 두 가지 불편한 점을 확인하였다. 
  1. 프로젝트를 재배포 하는 과정에서, 기존에 실행 중이던 도커 컨테이너를 중단한 뒤, 새로 받은 이미지의 컨테이너를 실행해야 하는 점.
  2. 위의 절차(gradle .jar build -> ...) 가 조금 번거롭다



첫 번째의 경우 '무중단 배포' 등의 개념으로 해결할 수 있을 것 같으며, 두 번째는 GitHub Action, Jenkins 등 CI/CD 도구를 사용해서 자동화 하는 것으로 대체할 수 있을 것으로 생각한다.

* 궁금증
  1. Dockerfile vs docker-compose.yml 의 차이는 무엇인가?





