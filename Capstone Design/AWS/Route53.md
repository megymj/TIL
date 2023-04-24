# AWS Route53



## 1. DNS와 Route53 개념

> [AWS Route 53을 이용한 도메인 적용](https://blog.bespinglobal.com/post/aws-route-53%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8F%84%EB%A9%94%EC%9D%B8-%EC%A0%81%EC%9A%A9/)

위 참고링크에 자세히 설명되어 있다. 

* AWS 한 줄 설명: Amazon Route 53는 가용성과 확장성이 뛰어난 [도메인 이름 시스템(DNS)](https://aws.amazon.com/route53/what-is-dns/) 웹 서비스입니다. Route 53는 사용자 요청을 AWS 또는 온프레미스에서 실행되는 인터넷 애플리케이션에 연결합니다.



## 2. 비용

도메인을 구매할 수 있는 곳은 국내에는 가비아가 대표적인 사이트이며, AWS Route 53에서도 1년 단위로 도메인 구매가 가능하다. [가비아: 도메인, 한 번 등록하면 영원이 내 것이 되는 것일까?](https://library.gabia.com/contents/domain/2853/) 를 참고하면, 가비아의 경우에도 도메인을 구매한 뒤, 몇 년마다 연장해야 한다. 

따라서, 이왕 AWS 서비스를 사용하여 아키텍처를 구성하므로, Route 53에서 도메인을 구매하려고 하였다. 



## 3. 도메인을 발급받는 원인



### 1. Google OAuth

* 현재 EC2에서는 3.34.14.220의 IP 주소를 사용하고 있었으나, Google OAuth에서 **OAuth 클라이언트 생성**할 때, URI 입력 과정에서 오류가 발생한다. 
  * OAuth 클라이언트 ID 및 보안 비밀번호를 발급받아야, 이후 application.yml 파일에 해당 정보를 등록하여 사용이 가능하므로, 이를 위해 발급받고자 하였다. 

<img width="609" alt="2  ec2" src="https://user-images.githubusercontent.com/80478750/233993446-48525901-49f9-4a28-b0e6-4e0cf699623f.png">
<img width="635" alt="1  local" src="https://user-images.githubusercontent.com/80478750/233993455-422ae554-b180-480b-83f4-9c170a4076a1.png">





### 2. Enable HTTPS

* 자세한 내용은 추후 기록
* certbot 등 무료 SSL 인증서가 있으나, AWS Route53 에서 제공하는 https 활성화를 사용할 예정

