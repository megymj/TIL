# Ubuntu gradle 설치

> https://linuxhint.com/installing_gradle_ubuntu/
>
> https://hoyhouse.tistory.com/8



### 1. Java 설치

* 기존에 Java 11이 설치되어 있어서 패스



### 2. Gradle 설치

* IntelliJ 프로젝트에서 gradle > wrapper > gradle-wrapper.properties 에서 현재 사용 중인 gradle 버전 확인
* https://gradle.org/releases/ 에서 gradle 버전 확인 가능

```bash
$ VERSION=7.5.1
$ wget https://services.gradle.org/distributions/gradle-${VERSION}-bin.zip -P /tmp
$ sudo unzip -d /opt/gradle /tmp/gradle-${VERSION}-bin.zip

# "sudo : unzip : command not found"라는 오류 메시지가 표시되면 unzip 패키지를 설치
$ sudo apt install unzip

# 심볼릭 링크 설정
$ sudo ln -s /opt/gradle/gradle-${VERSION} /opt/gradle/latest

# 환경 변수 설정
$ sudo vi /etc/profile.d/gradle.sh
# 편집기에 작성
export GRADLE_HOME=/opt/gradle/gradle-7.5.1
export PATH=${GRADLE_HOME}/bin:${PATH}

# Change the permissions of file
$ sudo chmod +x /etc/profile.d/gradle.sh
$ source /etc/profile.d/gradle.sh

# gradle version check
$ gradle -v
```





### 3. 오류 해결

> https://askubuntu.com/questions/554045/java-home-is-set-to-the-wrong-directory

위에서 `gradle -v` 명령어를 입력하였으나, 아래 사진과 같은 오류가 발생하였음

<img width="809" alt="error1" src="https://user-images.githubusercontent.com/80478750/231151293-cee62a25-63c7-4581-8bb6-74b0a11c7a7f.png">

* 위 링크를 참고해서 해결함

```bash
readlink -f $(which java)

# 위 명령어를 통해 경로를 확인하면, 해당 경로에서 bin/java를 제외한 경로를 아래에 추가한다
$ export JAVA_HOME=""

$ gradle -v
```

