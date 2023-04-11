# gradle project run in ubuntu

> [gradle 공식 문서: Java Application Plugin](https://docs.gradle.org/current/userguide/application_plugin.html)



### 1. Ubuntu에 gradle 설치

[내 GitHub > Ubuntu gradle 설치](https://github.com/Moojun/TIL/blob/main/Server/Ubuntu/Ubuntu%20gradle%20%EC%84%A4%EC%B9%98.md) 참고





### 2. gradle build, gradle clean, gradle run

* `gradle build` 의 경우 정상 수행 되는데, `gradle run` 명령어 입력 시 아래와 같은 오류 메시지가 출력되면서 실행이 되지 않았다. 

> FAILURE: Build failed with an exception.
>
> What went wrong: Task 'run' not found in root project ...



#### 2-1. 해결

* 프로젝트의 `build.gradle` 수정

```java
plugins {
    id 'java'
    id 'application'	// 추가
}

group 'org.selab'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'

    // lib 폴더 및 lib_LAS 폴더 내에 있는 *.jar 파일들 classpath 잡도록 추가
    implementation fileTree(dir: 'lib', include: ['*.jar'])
    implementation fileTree(dir: 'lib_LAS', include: ['*.jar'])
}

test {
    useJUnitPlatform()
}

// mainClass 명시 추가
application {
    mainClass = 'org.selab.Main'
}
```

