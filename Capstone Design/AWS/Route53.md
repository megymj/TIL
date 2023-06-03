# AWS Route53



## 1. DNS와 Route53 개념

> [AWS Route 53을 이용한 도메인 적용](https://blog.bespinglobal.com/post/aws-route-53%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8F%84%EB%A9%94%EC%9D%B8-%EC%A0%81%EC%9A%A9/)

위 참고링크에 자세히 설명되어 있다. 

* AWS 한 줄 설명: Amazon Route 53는 가용성과 확장성이 뛰어난 [도메인 이름 시스템(DNS)](https://aws.amazon.com/route53/what-is-dns/) 웹 서비스입니다. Route 53는 사용자 요청을 AWS 또는 온프레미스에서 실행되는 인터넷 애플리케이션에 연결합니다.



## 2. 비용

도메인을 구매할 수 있는 곳은 국내에는 가비아가 대표적인 사이트이며, AWS Route 53에서도 1년 단위로 도메인 구매가 가능하다. [가비아: 도메인, 한 번 등록하면 영원이 내 것이 되는 것일까?](https://library.gabia.com/contents/domain/2853/) 를 참고하면, 가비아의 경우에도 도메인을 구매한 뒤, 몇 년마다 연장해야 한다. 

따라서, 이왕 AWS 서비스를 사용하여 아키텍처를 구성하므로, Route 53에서 도메인을 구매하려고 하였다. 



## 3. 도메인 발급

* 현재 EC2에서는 3.34.14.220의 IP 주소를 사용하고 있었으나, Google OAuth에서 **OAuth 클라이언트 생성**할 때, URI 입력 과정에서 오류가 발생한다. 
  * OAuth 클라이언트 ID 및 보안 비밀번호를 발급받아야, 이후 application.yml 파일에 해당 정보를 등록하여 사용이 가능하므로, 이를 위해 발급받고자 하였다. 
  * 하지만, Public IPv4 address 뿐만 아니라, http://ec2-3-34-14-220.ap-northeast-2.compute.amazonaws.com 형식의 Public IPv4 DNS 주소를 EC2에서 제공하는 것을 확인하였으며, 위 DNS 주소를 사용하면 **OAuth Client** 를 생성할 수 있는 것을 확인하였다. 따라서 도메인 발급은 추후 진행할 예정

<img width="609" alt="2  ec2" src="https://user-images.githubusercontent.com/80478750/233993446-48525901-49f9-4a28-b0e6-4e0cf699623f.png">
<img width="635" alt="1  local" src="https://user-images.githubusercontent.com/80478750/233993455-422ae554-b180-480b-83f4-9c170a4076a1.png">

###  

### Domain 구매

* 1년 13$ + 1.5$(tax) = 14.5$
* [AWS docs: Routing traffic to an Amazon EC2 instance](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-to-ec2-instance.html) 참고해서 BE, FE EC2 domain 연결





## 4. Enable Https

>  [HTTPS 연결을 사용하여 내 웹 사이트에 액세스하려고 할 때 클라이언트에서 인증서 오류 메시지가 표시됩니다. 이 문제를 해결하려면 어떻게 해야 합니까?](https://repost.aws/ko/knowledge-center/acm-certificate-error-https)
>
>  https://velog.io/@server30sopt/EC2-HTTPS로-연결하기
>
>  [AWS Route 53 HTTPS 통신을 위한 SSL 인증서 발급하기 (Route 53, Certificate Manager, Load Balancer)](https://wooono.tistory.com/159)

certbot 등 무료 SSL 인증서가 있으나, AWS Route53 에서 제공하는 `ACM` 을 사용해서 인증



### 4-1. Route 53 도메인 등록

* 3번 과정에서 진행했으므로 pass



### 4-2. AWS Certificate Manager에서 SSL/TLS 인증서 받기

1. 도메인 이름 추가

2. 검증 방법 선택 → DNS 검증 선택

3. 태그 추가

4. 검토 및 요청

5. Route 53에서 레코드 생성 

   a. 이후 Route 53 > Hosted zones에 CNAME이  추가된다.





### 4-3. EC2 Target groups 생성

#### 주의] 생성 시 Port를 입력할 때, 현재 서버에서 동작 중인 Port를 입력해야 한다. 

* 예를 들어, Spring Boot 서버를 8080 포트로 실행 중이라면, 8080을 입력해야 한다. 

<img width="1072" alt="img1" src="https://github.com/Moojun/TIL/assets/80478750/ef24bbf8-2f0d-4bd1-bf90-90ad39c75920">



### 4-4. Security Group 생성

1. 80, 443 port 허용.
2. 타겟 그룹과 연결된 EC2 인스턴스의 보안 규칙에, ELB 보안 규칙 추가.





### 4-5. ELB 생성

- AWS Console → EC2 → 로드밸런싱 → 로드밸런서 → 로드밸런서 생성 → Application Load Balancer 생성
  - HTTP 및 HTTPS 트래픽을 사용하기 때문에 Application Load Balancer를 선택
- 80, 443 활성화



### 4-6. ELB 리스너 Redirection 수정

1. AWS Console → EC2 → 로드밸런서 → 생성한 로드밸런서 선택 → 리스너 탭 → HTTP : 80 리스너 선택

2. 규칙 부분 → 규칙 보기/편집 선택 → 편집/보기 페이지의 상단 편집 버튼 선택

3. THEN에서 기존 규칙 삭제 → 작업 추가 → 리디렉션 대상 → HTTPS, 443 포트를 입력 → 저장 → 업데이트





### 4-7.도메인에서 ELB로 트래픽을 라우팅하기 위해 Route 53 레코드 생성

#### 이 부분에서 한참을 해맸다. 처음에는 Record name 생성 시에 공백으로 설정해서 `.menjil-menjil.com` 으로 등록하였으나, https 접속 시 오류가 발생하였다.

##### Record name에 www를 붙이니 https로 접속이 잘 된다. 

<img width="477" alt="img22" src="https://github.com/Moojun/TIL/assets/80478750/d16028aa-3018-49fc-ae64-82ce234081d0">



#### 아마 아래 사진처럼 인증서가 서버 이름에 대해 유효하지 않은 경우인 것 같다.

<img width="884" alt="img341" src="https://github.com/Moojun/TIL/assets/80478750/81cdb90f-0862-423c-abd5-8cc0252ae870">



