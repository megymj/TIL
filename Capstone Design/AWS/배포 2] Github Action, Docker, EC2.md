# 배포 2] GitHub Action, Docker, EC2

* [Spring Boot 배포](https://github.com/Moojun/TIL/blob/main/Capstone%20Design/AWS/Spring%20Boot%20%EB%B0%B0%ED%8F%AC.md) 에서, Docker를 사용해 Docker Image를 Docker Hub에 올린 뒤, EC2에서 Docker Image를 pull로 받아온 뒤, 서버에서 실행하는 과정을 수행하였다.
* 그런데 매번 main branch로 커밋이 발생할 때마다, 위의 과정을 수행하는 것은 너무 번거롭다는 생각이 들었다. 그리고 현재에는 EC2를 배포용으로만 사용하고 있지만, 추후 develop용 EC2도 생성하게 된다면, develop branch에 커밋이 발생할 때마다 동일한 과정을 수행해야 한다는 불편함이 존재한다.



## 1. GitHub Action, Docker를 이용하여 EC2에 배포 자동화

> [GithubAction, Docker, EC2를 이용해 배포 자동화](https://velog.io/@donggni0712/GithubAction-Docker-Ec2를-이용해-배포-자동화)

<img width="694" alt="img1" src="https://user-images.githubusercontent.com/80478750/233302639-d389e7f9-e4b3-4f55-9918-e0b128114309.png">

### 1-1. CI/CD

> [Redhat; CI/CD(Continuous Integration/Continuous Delivery)란?](https://www.redhat.com/ko/topics/devops/what-is-ci-cd)



### CI(Continuous Integration)

- "지속적 통합" 을 의미하며 여러 개발자가 하나의 프로젝트를 같이 개발할 때 발생하는 불일치를 최소화 해주는 개념.
- CI 를 제대로 구현하면 애플리케이션 변경 사항 반영 시 자동으로 빌드 및 테스트 되어 변경 사항이 애플리케이션에 제대로 적용되었는지를 확인할 수 있다. 
- 또한 자동화된 테스트에서 기존 코드와 신규 코드 간의 충돌이 발견되면 CI를 통해 이러한 버그를 더욱 빠르게 자주 수정할 수 있다.



### CD(Continuous Delivery & Deployment)

* Continuous Delivery: CI의 빌드 자동화, 유닛 및 통합 테스트 수행 후, 이어지는 Continuous Delivery 프로세스에서는 유효한 코드를 리포지토리에 자동으로 릴리스한다.
* Continuous Deployment: 실제 사례에서 Continuous Deployment란 개발자가 애플리케이션에 변경 사항을 작성한 후 몇 분 이내에 클라우드 애플리케이션을 자동으로 실행할 수 있는 것을 의미한다(자동화된 테스트를 통과한 것으로 간주).



### What is GitHub Action?

> [GitHub Action docs](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions)
>
> [GitHub Action billing](https://docs.github.com/ko/billing/managing-billing-for-github-actions/about-billing-for-github-actions)

* GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. You can create workflows that build and test every pull request to your repository, or deploy merged pull requests to production.
* GitHub Actions goes beyond just DevOps and lets you run workflows when other events happen in your repository. For example, you can run a workflow to automatically add the appropriate labels whenever someone creates a new issue in your repository.
* 하지만 GitHub Action은 부분 무료이며, 두 번째 참고 링크를 확인하면, GitHub Free의 경우 매달 2,000 Minutes 사용은 무료이나, 그 이후는 비용이 과금된다. 





### GitHub Action 코드1: Gradle build & Docker Image build and push

* ${{ secrets.DOCKERHUB_USERNAME }} 에서 DockerHub에 등록된 닉네임의 대소문자를 잘못 입력하니 오류 발생.
* $(date +%Y-%m-%d-%H-%M-%S) 형식을 사용할까 하다가, ${GITHUB_SHA::7}를 사용하였는데, 어차피 Docker Push할 때 `:latest` 를 사용하므로 DockerHub에는 latest 태그로 이미지가 업로드된다. 

```yaml
# main.yml
name: Java CI with Gradle in main branch

on:
  push:
    branches: ["main"]

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    # workflow 실행 전 기본적으로 checkout 필요
    # 최신 버전은 v3
    - uses: actions/checkout@v3
    
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin' # jdk를 제공하는 vender사 이름 ex. zulu, adopt, microsoft

    ## Gradle
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
        
    - name: Build with Gradle
      run : ./gradlew clean build

    ## Docker
    - name: Build Docker image
      # run: docker build -t ${{ secrets.DOCKERHUB_REPO }}:$(date +%Y-%m-%d-%H-%M-%S) . # Use unix date format
      run: docker build -t ${{ secrets.DOCKERHUB_REPO }}:${GITHUB_SHA::7} .

    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push
      uses: docker/build-push-action@v3
      with:
        context: .
        file: ./Dockerfile
        platforms: linux/amd64
        push: true
        tags: ${{ secrets.DOCKERHUB_REPO }}:latest
```

<img width="908" alt="docker_img" src="https://user-images.githubusercontent.com/80478750/233789272-14129dcd-b8cb-4208-a7a1-3b26d25cdd2b.png">



### 문제점

* 여러 블로그들을 참고하여 위의 yaml 코드를 작성하였는데, 참고했던 모든 글에서 tag명을 `latest` 로 Docker Hub에 push 하도록 코드가 작성된 것을 확인하였다. 이 경우, Docker Hub에 업로드되는 image의 TAG가 모두 latest로 등록되므로, 구분하기 힘들다.

* 내가 생각한 해결 방안은 2가지가 있다.

  1. `GitHub Repository > Settings > secrets` 에, TAG 번호를 담을 변수를 하나 생성한 뒤, GitHub Action이 수행될 때 마다 숫자를 0.1 혹은 1 증가하게 하는 방식으로 구현
  2. yaml 코드 내에, 전역변수를 선언한 뒤, 해당 전역변수를 TAG 버전으로 사용하는 방식

  * 1번 보다는 2번 방식이 더 적절한 것으로 판단되어, 2번 방식으로 코드를 변경하고자 하였음.



### 시도한 코드

> [Learn GitHub Actions > Variables](https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables)
>
> [How to use GitHub Actions environment variables](https://snyk.io/blog/how-to-use-github-actions-environment-variables/)

* env: 에 전역변수 선언
* 결과: name `Build and push` 에서 오류 발생
  * 해결하기 위해 3시간 정도 구글링을 하였으나, 이와 관련된 내용을 찾지 못하였으며, 대다수 글에서는 tag를 `latest` 를 사용하는 것을 확인하였다. 
  * 생각해보니 어차피 프로젝트에서는 항상 최신 버전을 release하므로, tag를 `latest`로 사용해도 크게 문제는 없을 것으로 보이며, 그리고 Docker에서는 태그명을 생략하면 기본적으로 `latest`가 자동으로 붙도록 지정되어 있었다. 


```yaml
name: Java CI with Gradle in main branch

env:
  # $(date +%Y-%m-%d-%H-%M-%S) -> $(date +%Y%m%d%H%M%S) 변경
  TAG_VERSION: $(date +%Y%m%d%H%M%S)

# 생략

    ## Docker
    - name: Build Docker image
    	# 여기서 ${env_TAG_VERSION} 으로 하면 빌드 오류. ${{env.TAG_VERSION}} 사용해야 함
      run: docker build -t ${{ secrets.DOCKERHUB_REPO }}:${{env.TAG_VERSION}} . 

    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push
      uses: docker/build-push-action@v3
      with:
        context: .
        file: ./Dockerfile
        platforms: linux/amd64
        push: true
        
        # 여기서 오류 발생. ${env.TAG_VERSION}, ${{env.TAG_VERSION}} 둘다 format error
        # Error: buildx failed with: ERROR: invalid tag "***:$(date +%Y-%m-%d-%H-%M-%S)": invalid reference format
        # env.TAG_VERSION 으로 하면 Docker Hub에 문자열 그대로 tag가 반영됨
        tags: ${{ secrets.DOCKERHUB_REPO }}:${env.TAG_VERSION}
```



### 최종 성공 코드

> https://docs.docker.com/build/ci/github-actions/

* 위 링크를 참고하여 코드 양식 일부 변경함. 
* Build Success

```yaml
name: Java CI with Gradle in main branch

on:
  push:
    branches: ["main"]

permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    # workflow 실행 전 기본적으로 checkout 필요
    # 최신 버전은 v3
    - uses: actions/checkout@v3
    
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin' # jdk를 제공하는 vender사 이름 ex. zulu, adopt, microsoft

    ## Gradle
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew
        
    - name: Build with Gradle
      run : ./gradlew clean build

    ## Docker
    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}
        
    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2

    - name: Build and push
      uses: docker/build-push-action@v4
      with:
        context: .
        file: ./Dockerfile
        platforms: linux/amd64
        push: true
        # tags: ${{ secrets.DOCKERHUB_REPO }}:${{ github.sha }}  
        tags: ${{ secrets.DOCKERHUB_REPO }}:latest
```

