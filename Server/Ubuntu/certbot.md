# Certbot 재발급 및 Node.js 프로젝트에 올리기

certbot SSL 인증서가 90일이 지나 만료되어서, 갱신하고자 하였음

server: ubuntu 20.04

### 1. certbot 갱신

```bash
# 아래 command를 사용하면 모의로 renew를 할 수 있다고 해서, 아래 command를 실행함
sudo certbot renew --dry-run 
```
![1](https://user-images.githubusercontent.com/80478750/194314651-b2a25f2a-89cf-4ac3-af00-b00bce8acac9.png)


* 결과는 위와 같은 오류가 발생하였다.(하얀색로 칠해진 부분은 인증서를 신청한 도메인 주소) 당시 내가 처음 발급한 인증서가 잘 되지 않아서(1번) 인증서를 하나 더 추가로 발급하였는데(2번), 발급 이후 기존의 인증서를 제대로 제거하지 않아서 2개의 인증서가 뜨는 것 같다.

* 1번의 경우 ```/etc/letsencrypt/live/[domain]/ ```  경로로 이동하니 ssl 인증서와 관련된 파일이 존재하지 않았고, 따라서 제거하였다.

  * ```bash
    # 아래 명령어를 입력 후, 1번에 해당하는 링크를 선택하여 제거함
    sudo certbot delete
    ```

* 2번의 경우, ```The error was: expected /etc/letsencrypt/live ... to be a symlink``` 키워드로 검색을 해 보았으나, 정확한 원인을 찾지 못하였다.

* 따라서 ```sudo certbot delete``` command로 2번 역시 삭제한 뒤, 새로 발급받기로 결정하였다.



<br>



### 2. certbot 재발급

* https://certbot.eff.org/ 공식 사이트로 접속해서, `Web Hosting Project` 와 `Ubuntu 20` 을 선택한다.
* 1번부터 7번까지 그대로 따라한다. 나의 경우, 기존에 한 번 발급받은 이력이 있어서 그런지 6번까지는 추가적으로 설치되는 것이 없었다.
  * 7번에서 ```sudo certbot certonly --standalone``` 을 사용함
<img width="1104" alt="2" src="https://user-images.githubusercontent.com/80478750/194314786-e49a272b-3fd5-4cd6-973e-0847e9499715.png">


* 80번 port와 관련된 오류라서, iptime 관리자 페이지로 접속하니(192.168.0.1) 도메인을 적용할 웹의 External port가 80인 반면 Internal port가 3000으로 설정되어 있었다.
<img width="641" alt="3" src="https://user-images.githubusercontent.com/80478750/194314979-8a794298-18c7-489d-b33f-1d51bd9c4b26.png">

 
  * 현재 프로젝트 내 설정이, port 80에서 port 3000으로 포트포워딩 하도록 설정되어 있는 상태라서, 발급을 위해 임시로 Internal port를 80으로 변경한 뒤 다시 7번 명령을 수행하니 정상적으로 발급이 되었다.

  * ssl 인증 키가 발급되는 경로: ```/etc/letsencrypt/live/[domain]/```

* 이후 Node.js 프로젝트로 가서, app.js 파일 수정

```javascript
const http = require("http")
const https = require("https")
const fs = require("fs")
const app = express();

const options = {
     key: fs.readFileSync("/etc/letsencrypt/live/[domain]/privkey.pem"),
     cert: fs.readFileSync("/etc/letsencrypt/live/[domain]/cert.pem"),
     ca : fs.readFileSync("/etc/letsencrypt/live/[domain]/chain.pem")
};


const app = express()
app.use((req, res) => {
  res.send("Hello ~")
})

http.createServer(app).listen(80)
https.createServer(options, app).listen(443)
```

* 이후 실행하면 정상적으로 https를 통해 웹에 접속이 가능하다.

  

<br>

### 3. 권한 변경

* 현재 root user로 ssl 인증서를 발급 받았는데, "admin" user를 새로 생성한 뒤 'admin' user로 Node.js Express Project를 pm2를 사용하여 실행하고자 하였다.

* 하지만 `node app.js` 명령어로 실행한 결과 아래와 같은 오류 메시지가 출력되었는데, 아마도 *.pem key가 권한이 root에게 있어서 오류가 발생한 것으로 보인다.

  * ```bash
    Error: EACCES: permission denied, open '/etc/letsencrypt/live/[domain]/privkey.pem'
    ```

* [stackoverflow](https://stackoverflow.com/questions/48078083/lets-encrypt-ssl-couldnt-start-by-error-eacces-permission-denied-open-et) 링크를 참고하여, 아래 명령어를 입력한 뒤 admin으로 실행하니 정상적으로 admin user에서 실행이 잘 되는 것을 확인하였다.

  * ```bash
    sudo chown admin -R /etc/letsencrypt
    ```

    

<br>



### 4. renew test

* admin user로 아래 명령어를 실행하니 갱신이 가능하다는 결과가 출력됨 (**단, 이 때도 iptime 관리자 페이지로 접속해서 Internal port number를 80으로 변경해줘야 한다**)
* root의 경우 아래 명령어를 실행하면 오류가 발생하는데, 아마 3번에서 ssl 파일의 권한을 admin에게 양도했기 때문에 발생하는 것 같다. 필요하다면 권한을 다시 root로 변경 후 아래 명령어를 입력하면 마찬가지로 갱신이 가능하다는 결과가 출력될 것으로 예상된다.

```bash
sudo certbot renew --dry-run 
```
