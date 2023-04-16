# Spring Boot 배포

> [참고링크1](https://bcp0109.tistory.com/356)
>
> [참고링크2](https://develop-writing.tistory.com/121)

특히 참고링크 2처럼, 배포 스크립트를 .sh 파일로 만들어서, 변경 사항이 발생할 때 마다 git pull, ./gradlew build 등의 명령어를 한 번에 수행할 수 있도록 만들 필요가 있다. 





### 1. Docker

> [도커를 쓰는 이유](https://www.44bits.io/ko/post/why-should-i-use-docker-container)
>
> [Top 5 Advatages of using Docker](https://www.cloudtern.com/cloud/top-5-advantages-of-using-docker/)
>
> [Why should I use docker with ec2? Why not ec2 alone?](https://www.reddit.com/r/aws/comments/c4cnpz/why_should_i_use_docker_with_ec2_why_not_ec2_alone/)



#### 도커의 장점

1.  Consistent Environment: Dockerfile을 통해 서버 운영기록을 코드로 남길 수 있다. 또한, 서로 다른 환경에서 Dockerfile을 통해 동일하게 실행될 수 있다.-&gt; 이는 팀원이 협업할 때, 프레임워크 버전 등 여러 버전을 일치시킬 수 있어서 쉬울 것으로 보인다. 
2. 서버의 확장: 만약 프로젝트 규모가 커져서 서버를 옮기거나 확장해야 할 때, 도커를 사용하면 Docker Image만 가져와 새로운 서버에 동일한 환경을 쉽게 구축할 수 있다. 
3. 그 외 이유들은 위 링크 혹은 구글링 참고



#### 개인 프로젝트에서 Docker를 사용해야 하는 이유?

현재 프로젝트에서 Docker가 사용되는 곳은 크게 두 가지임(추후 추가될 수 있음)

	1. EC2에 Spring Boot 배포 
	1. 문장 유사도 검증시 AWS Lambda를 사용하는데, 이 때 python code를 Docker image로 업로드

이 중 2번의 경우, 여러 머신러닝 라이브러리를 코드에서 사용하기 때문에 Docker를 사용하는 것이 거의 반강제이나, 1번의 경우 실제 배포해서 창업을 위한 프로젝트가 아닌 졸업 작품 프로젝트이므로 EC2 Instance를 하나 이상 사용할 것 같지 않다. 따라서 EC2 인스턴스에 초기 환경 구축(Java 11 설치 등)을 한 이후, 환경이 수정될 것 같지는 않지만, 원활한 관리 및 추후 서버 확장 가능성을 고려하여 Docker를 사용하기로 하였다. 



### 2. Spring Boot + Docker + EC2

