# AWS] EC2에 node.js 배포하기
* ssh를 통해 cmd창에서 EC2 instance로 접속
* EC2 instance type: `Ubuntu Server 20.04`

## 1. PPA를 이용해서 Node.js 설치
> reference link: [velog](https://velog.io/@ywoosang/Node.js-%EC%84%A4%EC%B9%98)

```bash
$ cd ~
$ curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
$ sudo bash nodesource_setup.sh 
$ sudo apt-get install nodejs
$ sudo apt-get install build-essential
```

<br>

## 2. Node.js 서버 구축
> reference link: [velog](https://velog.io/@limsw/AWS-EC2%EC%97%90-Node.js-%EC%84%9C%EB%B2%84-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0)
```bash
# Express 환경 구성
$ npm init
$ npm install express —-save
```

index.js 파일을 만들고 아래와 같이 코드 작성
```javascript
const express = require('express');
const app = express();

app.get('', (req, res, next) => {
    res.send("Hello world\n");
});

app.listen(3000, () => {
    console.log("App is listening 3000 port");
});
```

<br>

## 3. instance에서 server 실행
index.js 파일이 있는 위치로 이동 후, `node index.js` 입력. "ctrl + c"를 통해 빠져 나갈 수 있다.
![image](https://user-images.githubusercontent.com/80478750/174535020-f59b893f-e3cf-4470-bfda-8f2988416838.png)


<br>

## 4. AWS Security Group
EC2 Instance의 inbound rules에서, 3000번 port를 열어 줘야 한다. 

