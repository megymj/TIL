## EC2 instance에 Nodejs 설치 및 GitHub의 Nodejsproject clone한 뒤 실행하기
EC2 instance: Ubuntu 20.04

> reference link: [tistory blog](https://soojae.tistory.com/25)

```bash
# 처음에 sudo apt install npm 명령을 통해 npm 먼저 설치하려고 하였으나, 설치가 되지 않음(오류 메시지 출력)

# install crul
$ sudo apt-get install curl

# add ppa
$ curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -

# install NodeJS
$ sudo apt-get install -y nodejs

# add build-essential
# PPA를 통해서 NodeJS를 설치하면 NodeJS 뿐만 아니라 npm도 같이 설치된다. 하지만 npm install시 에러가 나는 것을 방지하여 build-essential을 설치.
$ sudo apt-get install build-essential

# 이후 clone한 project directory로 이동해, Express 설치
$ npm install express

# run
$ node app.js

```
