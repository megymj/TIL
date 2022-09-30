# AWS EC2에 Node.js Express 프로젝트 올리기

<br>

## 0. 실행 방법

* EC2 instance 접속: EC2 key chain 위치에서. ssh로 접속(Inbound rules: ssh 22 port open 필요)

```bash
# Nodejs project run
$ cd /exp
$ cd /NodeProject
$ npm start

로컬에서 [EC2 public ip]:3000 접속

# MySQL 접속(EC2)
$ mysql -u [user name] -p -h [RDS Endpoint]

# MySQL 접속(local MySQL Workbench)
Connection Name: 임의로 지정
Hostname: [RDS Endpoint]
Username: 
Password: 
Port: 3306
입력 후 Test Connection
```



<br>



## 1. IAM Group 생성 및 User 생성 

AWS IAM Service 이용

<br>



## 2. EC2 instance 생성

* EC2 Key pairs 생성 및 instance 생성(AMI: Ubuntu 20.04)
* Elastic IP 생성 및 EC2 instance와 연결
  * Elastic IP: 고정 IP 할당 -> EC2를 stop 및 restart 시 IP address가 변경되지 않음

* SSH로 EC2 instance 접속 후 개발환경 setting

```bash
# 개발환경 setting

sudo apt-get update

sudo apt-get install git
git --version

sudo apt-get install -y build-essential
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
sudo apt-get install -y nodejs
sudo apt install npm
node -v
npm -v

# For private repository
git config --global user.name "[user name]"
git config --global user.mail "[user email]"
# git config --list

git clone [repository url]
# id, token 입력


# Install MySQL
# reference link: https://liveloper-jay.tistory.com/31
sudo apt install -y mysql-server
# run mysql
# sudo /usr/bin/mysql -u root -p
# password는 따로 설정하지 않았으므로 enter를 누르면 mysql로 접속이 된다.

# install Express
npm install express-generator 
npm install
npm start

# install pm2
sudo npm install pm2 -g
pm2 start app.js
# pm2 start app.js --name "[name]" : [name] 대신 원하는 이름으로 지정 가능
# pm2 list : pm2 목록 보기
# pm2 stop [name] : process 중단
# pm2 delete [name] : process 종료
# pm2 kill: pm2 종료

```

<br>

## 3. AWS RDS 생성 및 EC2와 연결

* RDS 생성 이후 Security group EC2와 연결. default port: 3306
* 생성 이후 Security group > Inbound rules에서 3306 port 활성화

* EC2에서 RDS MySQL 접속 command

  ```mysql
  mysql -u [user name] -p --host [RDS Endpoint]
  ```


<br>


## 4. MySQL Data import to RDS

* mysqldump, filezilla clinet 사용해서 옮김

* server에서 mysqldump command로 .sql 백업 파일들 생성 이후, filezilla clinet를 통해 local에 저장.

* 이후 filezilla를 통해 Local에서 EC2에 접속한 뒤, .sql 백업 파일들 업로드

* 여기서 **주의**, .sql 백업 파일들이 백업되는 위치는 EC2 내의 MySQL이 아닌, RDS MySQL임. 따라서, mysqldump command 일부 수정 필요

  * 시도2: [reference link](https://www.phpschool.com/gnuboard4/bbs/board.php?bo_table=qna_db&wr_id=199595)

  ```sql
  # 시도1
  mysqldump -h [RDS Endpoint] -u [user name] -p [database name] < *.sql
  
  # 시도2: 시도1이 안 먹는 경우
  # *.sql 파일 경로에서 실행해야 함
  mysql -h [RDS Endpoint] -u [user name] -p [database name] -e "source /디렉토리 위치/*.sql"
  ```


<br>


## 4-1. local에서 AWS RDS에 접속 안되는 오류

로컬의 MySQL Workbench에서 RDS database를 접속하려 하였으나, 접속이 되지 않음

이를 해결하기 위해,

1. database 설정해서, **Publicly accessible** 선택

   <img width="819" alt="Screen Shot 2022-08-06 at 8 21 27 PM" src="https://user-images.githubusercontent.com/80478750/183567995-d12a3c7a-7978-4abe-9f15-05ba1d5846e8.png">

2. RDS VPC Security Group에서, inbound rules에 MySQL 3306 port 추가

-> 이후에도 되지 않았으나, database 생성 후 이틀 뒤에, 추가적으로 아무것도 건드리지 않았음에도 불구하고 로컬에서 접속이 되었다.



<br>



## +a] 2번에서 EC2 instance에 git clone시 궁금증

* git clone의 경우 public repository에서는 config 없이도 동작하나, private repository에서는 config해야 동작한다고 함
* 그런데, private repository를 ec2 instance로 clone할 때, 내 계정으로 clone하면 이후 해당 private repository에 다른 참가자들은 clone된 private repository에 접근할 때 어떤 방식으로 접근이 되는 것인가? 
  * 내 계정을 덮어쓰는 방식인가? 
  * 혹은 clone된 private repository를 push, pull 등의 기타 작업을 수행할 때마다 매번 id와 token을 입력하라고 창이 뜨는 것인가? 
  * ~~아직 잘 모르겠다.~~   -> push의 경우는 잘 모르겠으나, pull을 할 때는 매번 id와 Token을 입력하라는 창이 뜬다.




