# mongoDB in Ubuntu 20.04

```bash
# 터미널 창에 아래 명령어를 입력해서, 현재 설치된 Ubuntu version 확인
lsb_release -dc
```



### 1. MongoDB 기본 명령어

```bash
# version check
mongod --version

# check if mongoDB activate
sudo systemctl status mongod

sudo systemctl start mongod
sudo systemctl status mongod
sudo systemctl stop mongod 
sudo systemctl restart mongod
mongo # Activation mongo instance(접속해서 작업 시작)

exit # 종료 명령어 (mysql은 quit이 가능한데, 여기는 quit 안먹힘)

# Activate port number check
sudo netstat -tnlp
```





### 2. Mongodb 오류 해결

#### 오류 1: "path":"/tmp/mongodb-27017.sock","error":"Operation not permitted

```bash
cat /etc/mongod.conf

# 실행 결과
# log 저장되는 path 확인
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log


tail /var/log/mongodb/mongod.log

# 실행 결과
{"t":{"$date":"2023-03-16T14:17:02.680+09:00"},"s":"E",  "c":"NETWORK",  "id":23024,   "ctx":"initandlisten","msg":"Failed to unlink socket file","attr":{"path":"/tmp/mongodb-27017.sock","error":"Operation not permitted"}}
```

<br>



> 참고 링크: https://stackoverflow.com/questions/29813648/failed-to-unlink-socket-file-error-in-mongodb-3-0

```bash
# 위 링크에서, /tmp/mongodb-27017.sock 파일의 접근 권한을 변경하거나 파일 삭제 후 서비스를 재시작하면 정상적으로 실행된다고 함
# 파일 삭제.
sudo rm -rf /tmp/mongodb-27017.sock

# 이후 mongodb를 재시작하니, 정상적으로 root 계정에서 접속이 되는 것을 확인함.
mongo
```



#### 오류 2. "attr":{"error":"IllegalOperation: Attempted to create a lock file on a read-only directory: /data/mongodb"}}

```bash
cat /etc/mongod.conf

stoarge:
	dbPath: /data/mongodb
```

에서, 아래 명령을 통해 권한을 준다. (Permission settings)

```bash
sudo chown -R mongodb: /data/mongodb/*
```



#### 3. port 번호를 변경한 경우

> 참고링크: [How to change mongoDB Default Listening port(27017)](https://medium.com/mongoaudit/how-to-change-mongodb-default-listening-port-27017-92e35f65670e)

* mongoDB의 기본 port는 27017이지만, `/etc/mongod.conf` 에서 port번호를 8808로 변경하였다. 

```bash
net:
  port: 8808
  bindIp: 0.0.0.0
  #bindIp: 127.0.0.1
```



이후 mongo를 재시작한 뒤 `mongo` command를 입력하였으나 exit 1 오류, 즉 실행되지 않는 오류가 발생함. 아래 로그들은 마지막 로그이나, 오류가 발생하였다는 사실은 찾을 수 없었다. 

```bash
{"t":{"$date":"2023-03-16T18:40:00.853+09:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"/tmp/mongodb-8808.sock"}}
{"t":{"$date":"2023-03-16T18:40:00.853+09:00"},"s":"I",  "c":"NETWORK",  "id":23015,   "ctx":"listener","msg":"Listening on","attr":{"address":"0.0.0.0"}}
{"t":{"$date":"2023-03-16T18:40:00.853+09:00"},"s":"I",  "c":"NETWORK",  "id":23016,   "ctx":"listener","msg":"Waiting for connections","attr":{"port":8808,"ssl":"off"}}
```

##### 해결 방법

* 위의 참고링크를 보고 해결함
* `mongo` 대신 `mongo --port 8808` 을 실행한다. 



### 4. 외부 접근 권한 허용

`/etc/mongod.conf` 파일 내부에서, 아래 내용 추가

```bash
security:
  authorization: 'enabled'
```

* 이 경우, mongodb에 계정이 존재해야 하며, mongodb는 아래 명령어로 접근이 가능하다.

  ```bash
  # 아래 명령어로 접속은 가능하나, 권한이 없으므로 show dbs 등의 커맨드를 입력하면 결과가 나오지 않는다.
  mongo --port 8808
  
  # 접속 시 유저 명칭을 작성해야 함
  mongo admin -u [username] -p [userpassword] --port 8808
  ```

* 하지만, macOS에서 접속하려고 robo-3t를 통한 연결 및 command를 이용해서 연결을 시도하였으나 동작하지 않음(연결되지 않음): `추후 해결될 시 작성 예정`

  ```bash
  # command
  mongosh -u [user_name] -p --host 117.17.198.22 --authenticationDatabase admin
  ```
  
  







### 5. MongoDB 명령어

> 참고링크: [mongodb docs](https://www.mongodb.com/docs/manual/core/databases-and-collections/)

#### 1] DB

* In MongoDB, databases hold one or more collections of documents.

```bash
# 생성된 db 리스트 조회
show dbs

# 현재 사용 중인 db 보기
db

# db 생성 및 선택
use [db_name]

# 현재 사용 중인 db 삭제
db.dropDatabase()
```



#### 2] Collection

* MongoDB stores documents in collections. Collections are analogous to tables in relational databases.

```bash
# 생성된 collection 조회
show collections
```



#### 3] Documents

* MongoDB stores data records as BSON documents. BSON is a binary representation of [JSON](https://www.mongodb.com/docs/manual/reference/glossary/#std-term-JSON) documents, though it contains more data types than JSON.

```bash
# Collection 내 모든 Document 조회
db.[collection_name].find()

# json pretty로 깔끔하게 출력
db.[collection_name].find().pretty()
```






