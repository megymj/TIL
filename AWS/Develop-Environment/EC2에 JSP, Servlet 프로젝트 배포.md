# EC2에 Tomcat Servlet 프로젝트 배포

> 참고 링크
> 1. [tomcat 설치](http://www.gnujava.com/board/article_view.jsp?&article_no=2439&menu_cd=19&board_no=6&table_cd=EPAR02&table_no=02)



<br>



### 0. EC2 Instance 생성 및 Elastic IP 생성 후 EC2에 연결

EC2 AMI: `Ubuntu 20.04`



<br>




### 1. EC2 기본 설정

EC2 인스턴스에 접속한 뒤, 아래 작업들을 진행

#### 1-1. Java 설치

* Tomcat 설치를 위해서는, Java가 먼저 설치되어 있어야 함

```bash
$ sudo apt-get update
$ sudo apt-get upgrade

# java-11 설치
$ sudo apt-get install openjdk-11-jdk

# 설치 확인
$ java -version
$ javac -version

# 환경변수 설정
## javac 위치 확인
$ which javac

## javac 위치 확인
$ readlink -f /usr/bin/javac
```



#### 1-2. Java 환경변수 등록(생략)

```bash
$ vi /etc/profile
```

위의 명령어로 profile 파일을 연 뒤, 내용을 추가적으로 입력해야 하지만 본 과정에서는 생략하였음



#### 1-3. Tomcat 설치 

Tomcat 공식 홈페이지에서 Tomcat 9.0 tar.gz 링크 복사

```bash
$ wget [복사한 주소 붙여넣기]

# 압축 풀기
$ tar xvfz apache-tomcat-9.0.70.tar.gz

# tomcat9.0 폴더를 만들고, 파일 이동
$ sudo mv apache-tomcat-9.0.70 /usr/local/tomcat9.0

# 환경변수 설정 후 아래의 export 내용을 작성
## 맨 아래에 작성
$ sudo vi /etc/profile

export CATALINA_HOME=/usr/local/tomcat9.0

# 적용
$ source /etc/profile

# 확인
$ echo $CATALINA_HOME
```



#### 1-4. Tomcat 설정(server.xml)

```bash
# server.xml 파일 위치로 이동
$ cd /usr/local/tomcat9.0/conf
$ ls
$ vi server.xml
```

URIEncoding="UTF-8" 을 추가. port 번호 8080이 맞는지 확인

<img width="550" alt="4번" src="https://user-images.githubusercontent.com/80478750/206897493-97867577-cfff-4b89-95ec-29427e0e13a9.png">



#### 1-5. Tomcat 실행 테스트

```bash
# 실행
$ sudo /usr/local/tomcat9.0/bin/startup.sh

# 종료
$ sudo /usr/local/tomcat9.0/bin/shutdown.sh
```



### 1-6. #주의사항: EC2 Ubuntu환경에서 시간 설정 필요

```bash
# 현재 시간 확인
# UTC로 설정되어 있는 것을 확인할 수 있다. 변경해 줘야 한다.
$ date
```

#### Ubuntu Timezone 변경하기

```bash
$ sudo dpkg-reconfigure tzdata
```

방향키를 사용하여 선택할 지역을 찾은 뒤, Enter 버튼을 클릭한다.

<img width="752" alt="1" src="https://user-images.githubusercontent.com/80478750/207247245-6836fb0a-d053-471a-bff4-a47a4d200e78.png">

<img width="707" alt="2" src="https://user-images.githubusercontent.com/80478750/207247239-c5916433-beaa-4e67-8c9f-da9c392d7f67.png">



이후 다시 현재 시간을 확인하면 정상적으로 변경되어 있는 것을 확인할 수 있다.

**만약 tomcat 서버가 실행 중이라면, 종료 후 재실행시 반영이 된다.**

```bash
# 현재 시간 확인
$ date
```



<br>



### 2. RDS 생성

Id: admin, pw: admin123å

mysqldump를 위해 우선 `Publicly accessible `로 설정하였음. 추후 `Not publicly accessible` 로 변경 필요

<img width="882" alt="1" src="https://user-images.githubusercontent.com/80478750/208129131-3b5e93d8-a846-44ea-9769-46ad3a464897.png">





#### 2-1. mysqldump를 사용하여 기존 local MySQL에 있는 database를 RDS endpoint로 복원

```bash
# macOS MySQL 위치로 접근
cd /usr/local/mysql/bin

# mysqldump 사용; 경로를 지정해 줘야 함. 경로를 지정하지 않고 그냥 stock.sql로 하면 오류 발생
mysqldump -u root -p Stock24 > ~/Desktop/stock/stock.sql

# RDS에 data backup
cd ~/Desktop/stock
mysql -h [RDD endpoint] -u admin -p stock24 < stock.sql
```



#### 2-2. db.properties 파일의 정보를 RDS 정보로 변경

db.properties 파일 정보

```java
url = jdbc:mysql://[RDS endpoint]/[database name]?useUnicode=true&characterEncoding=utf8
username = [RDS id]
password = [RDS password]
```



### 2-3. # 주의사항: RDS를 사용할 때, time zone을 변경해 줘야 한다.

```sql
# 현재 db 시간 정보 확인
mysql > SELECT NOW();
```

현재 시간을 확인하게 되면, 대한민국 시간대는 UTC +9:00이므로, 현재 한국 시간보다 9시간 이전 시간으로 뜬다. 이것을 한국의 현재 시간대로 설정해줘야 한다. 



#### 1] RDS > Parameter groups로 가서 새로운 parameter group을 생성한다

default 그룹의 경우 수정이 불가능하다고 한다.

<img width="1391" alt="pgroup" src="https://user-images.githubusercontent.com/80478750/207244038-a40911bf-b516-4049-af30-7384a31fd527.png">



#### 2] RDS > Parameter groups로 가서 새로운 parameter group을 생성한다

<img width="886" alt="2" src="https://user-images.githubusercontent.com/80478750/208129477-77565179-e0fc-47f6-b17b-f675c8e10d49.png">



#### 3] 파라미터 그룹 생성 이후, time_zone을 검색해서, Asia/Seoul로 변경

<img width="731" alt="3" src="https://user-images.githubusercontent.com/80478750/208129877-6047582d-bdae-4826-9055-3e6204cd6209.png">



#### 4] 변경한 뒤 기존의 데이터베이스 Modify를 클릭한 뒤 parameter group을 지정

<img width="756" alt="4" src="https://user-images.githubusercontent.com/80478750/208129875-9e8dd729-0e12-42dd-a701-91f5a410faee.png">

#### 5] Reboot 실행

Reboot를 한 뒤에야 적용이 된다고 한다. 

<img width="1071" alt="5" src="https://user-images.githubusercontent.com/80478750/208129870-3c621cda-c194-494c-94ea-99f3154d3d37.png">



#### 6] Test

정상적으로 시간대가 변경된 것을 확인함.

<img width="625" alt="6" src="https://user-images.githubusercontent.com/80478750/208130238-7b3a1528-ef61-4235-857e-9fccb4829190.png">





<br>



### 3. Intellij 프로젝트에서 .war 파일 추출

> 참고: [war file export](https://xzio.tistory.com/1268)





<br>



### 4. EC2에 설치된 Tomcat 디렉토리에 .war 파일 배포

Filezila clinet를 사용하여 EC2 인스턴스와 연결한 뒤, EC2 내의 Tomcat9.0의 webapps 에 3번에서 추출한 .war 파일을 위치시켜 주면 된다. webapps 밑에 .war 파일을 넣어두면 된다.

연결하는 방법은 두 가지가 있는데, 나는 첫 번째 방법을 사용하였다.

1. webapps 밑에 있는 ROOT 폴더를 지우고 배포한 war 파일의 파일명을 ROOT.war로 변경한다. 이후 1-5번 명령을 사용해서 Tomcat을 실행한다.
2. war 파일명을 내가 지정한 명칭으로 사용하는 방법: 상세한 방법은 생략(추후 작성 예정)



#### 4-1. 서버 실행
```bash
# 실행
$ sudo /usr/local/tomcat9.0/bin/startup.sh

# 종료
$ sudo /usr/local/tomcat9.0/bin/shutdown.sh
```




<br>



### 5. AWS 추가 설정: Public/Private Subnet 분리



### 5-1. RDS 외부 접속 차단

<img width="798" alt="7" src="https://user-images.githubusercontent.com/80478750/208231225-e5f213c3-659f-4673-8b9c-80238c9d4cdd.png">





#### 5-2. RDS를 위한 Security Group 새로 생성

* 생성 후 기존 EC2와의 연결을 위해 EC2의 보안 그룹을 지정한다. 

<img width="1173" alt="8" src="https://user-images.githubusercontent.com/80478750/208231306-c6e81039-6f42-4509-90a2-9abcbf26354f.png">



* VPC의 보안 그룹을 변경한다.

<img width="772" alt="9" src="https://user-images.githubusercontent.com/80478750/208231301-d0aac28c-8c5f-44dc-abdd-fb573f01402e.png">





#### 5-3. EC2에 mysql 설치 및 접속

```bash
$ sudo apt update

# install
$ sudo apt install mysql-client

# connect RDS
$ mysql -u [RDS id] -p -h [RDS EndPoint]
```





