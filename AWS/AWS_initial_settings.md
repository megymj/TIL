# AWS 시작 시 설정하면 좋은 것들
## 1. 계정 Multi-factor Authentication(MFA) 설정
* AWS에는 Root 계정과 IAM 게정이 있는데, Root계정에서 MFA 설정은 필수적이다. 
* 검색 결과 실제로 MFA 설정을 하지 않아서, 해킹으로 인해 AWS 요금이 내가 사용한 것 이상으로 나온 경우가 종종 있다고 한다.
1. AWS 로그인 이후, 내 게정을 클릭하여 `내 보안 자격 증명`을 클릭한다.
2. `멀티 팩터 인증 -> MFA  활성화` 
3. 이후 핸드폰에 google OTP 어플을 설치한 뒤, 인증 절차를 거치면 된다.

<br>

## 2. Free Tier(프리 티어) 사용량 알림 받기
* [AWS 참고 링크](https://aws.amazon.com/ko/about-aws/whats-new/2017/12/aws-free-tier-usage-alerts-automatically-notify-you-when-you-are-forecasted-to-exceed-your-aws-service-usage-limits/)
* AWS Free Tier는 무료이지만, 일정 용량 이상을 사용하면 과금이 되는 것으로 보인다. 
* 따라서, Free Tier 사용량 알림을 신청해 놓는 것이 좋다.
1. AWS 로그인 이후, console 검색 영역에 `결제 방법` 검색 후 선택
2. `결제 기본 설정 -> 프리 티어 사용량 알림 받기` 체크 후 이메일 작성   
3. `기본 설정 저장` 클릭.

아래 사진은 사용량 알림을 설정하지 않아, 과금이 된 이후에 확인을 하였다. EC2 인스턴스는 정상적으로 제거했으나, 탄력적 IP를 릴리스하지 않아서 요금이 부과되었음
![탄력적IP](https://user-images.githubusercontent.com/80478750/161408428-7dbb2e10-c20c-463f-a894-0a6adcd76586.PNG)
