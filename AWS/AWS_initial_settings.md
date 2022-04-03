# AWS 시작 시 설정하면 좋은 것들

## 1. Free Tier(프리 티어) 사용량 알림 받기
* [AWS 참고 링크](https://aws.amazon.com/ko/about-aws/whats-new/2017/12/aws-free-tier-usage-alerts-automatically-notify-you-when-you-are-forecasted-to-exceed-your-aws-service-usage-limits/)
* AWS Free Tier는 무료이지만, 일정 용량 이상을 사용하면 과금이 되는 것으로 보인다. 
* 따라서, Free Tier 사용량 알림을 신청해 놓는 것이 좋다.
1. AWS 로그인 이후, console 검색 영역에 `결제 방법` 검색 후 선택
2. `결제 기본 설정 -> 프리 티어 사용량 알림 받기` 체크 후 이메일 작성   
3. `기본 설정 저장` 클릭.

아래 사진은 사용량 알림을 설정하지 않아, 과금이 된 이후에 확인을 하였다. EC2 인스턴스는 정상적으로 제거했으나, 탄력적 IP를 릴리스하지 않아서 요금이 부과되었음
![탄력적IP](https://user-images.githubusercontent.com/80478750/161408428-7dbb2e10-c20c-463f-a894-0a6adcd76586.PNG)