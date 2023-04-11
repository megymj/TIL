# AWS 시작 시 설정하면 좋은 것들



### 1. 계정 Multi-factor Authentication(MFA) 설정
* AWS에는 Root user와 IAM user가 있는데, Root user에서 MFA 설정은 필수적이다. 
* 검색 결과 실제로 MFA 설정을 하지 않아서, 해킹으로 인해 AWS 요금이 내가 사용한 것 이상으로 나온 경우가 종종 있다고 한다.
1. AWS 로그인 이후, 내 계정 > [Security credentials](https://console.aws.amazon.com/iam/home#security_credential)을 클릭한다.

2. `멀티 팩터 인증 -> MFA  활성화` 

   <img width="1195" alt="img1" src="https://user-images.githubusercontent.com/80478750/230595310-a45ece9f-0911-4294-a42a-b9120260dcda.png">

3. 이후 핸드폰에 Google Authentication 등의 어플을 설치한 뒤, 인증 절차를 거치면 된다.

<br>

### 2. Free Tier(프리 티어) 사용량 알림 받기
* [AWS 참고 링크](https://aws.amazon.com/ko/about-aws/whats-new/2017/12/aws-free-tier-usage-alerts-automatically-notify-you-when-you-are-forecasted-to-exceed-your-aws-service-usage-limits/)
* AWS Free Tier는 무료이지만, 일정 용량 이상을 사용하면 과금이 발생한다. 단적인 예로, EC2 free tier의 경우 한 달에 750시간 동안 무료지만, EC2를 두 개를 띄어놓으면 과금의 가능성이 존재한다. 따라서, Free Tier 사용량 알림을 신청해 놓는 것이 좋다.
1. AWS 로그인 이후, console 검색 영역에 `결제 방법` 검색 후 선택
2. `결제 기본 설정 -> 프리 티어 사용량 알림 받기` 체크 후 이메일 작성   
   * 혹은 Acoount > `Billing preferences` 선택
3. `기본 설정 저장` 클릭.
