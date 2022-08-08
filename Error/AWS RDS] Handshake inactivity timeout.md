# AWS RDS] Handshake inactivity timeout

EC2에서 Nodejs Express 프로젝트를 실행하는데 아래 사진과 같은 오류가 발생함
![error](https://velog.velcdn.com/images/moojun3/post/a42d23e3-1f4b-447e-ac21-7edd3ec3d9c2/image.png)

해결: RDS의 Security group의 Inbound 규칙이 특정 IP로 되어있었다. Inbound rules에서 3306 MySQL port를 모든 IPv4로 열어주니 해결됨