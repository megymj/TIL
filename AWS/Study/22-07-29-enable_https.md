# 07.29 https 연결 활성화

참고 블로그: [1](https://devlog-wjdrbs96.tistory.com/293), [2](https://webruden.tistory.com/698), [3](https://happy-jjang-a.tistory.com/102)



AWS에서 domain은 이미 구매한 상태.



1. AWS Console > Certificate Manager(acm) 에서, `request a certificate` 클릭

   * Route 53에 등록된 도메인 이름을 입력하고, 요청 클릭
   * 이후, 발급된 인증서 클릭 후 상세페이지에서 오른쪽에 `Route53에서 Record 생성` 클릭
   * 목록에서 인증서 발급 확인

   

2. 발급된 인증서 ELB에 연결

   * ELB 화면에서, NL-AWS-ALB(대상 ELB) 클릭하고 '리스너 > 리스너 추가'
   * HTTPS protocol 선택 후, '1.다음으로 전달' 에서 대상 그룹 지정
   * '보안 리스너 설정'에서 기본 SSL 인증서 > ACM에서 > '1번에서 발급된 인증서 선택'
     * 주의: 발급된 인증서가 선택되지 않는다면, 새로고침을 누르기




3. 리스너 생성 확인 및 Security Group InBound 규칙에 HTTPS(443) 추가
   * 현재 NL-AWS-ALB에 연결된 보안 그룹은 launch-wizard-3과 launch-wizard-4인데, launch-wizard-3의 InBound 규칙을 확인하면, 전체 port에 대해 접속이 허용되어 있어서, 따로 수정 없이 https로 바로 접속이 가능하다



