# GitHub의 Pull Request와 Issue
GitHub에서 자주 사용되는 Pull Request(PR)과 Issue에 대해 알아본다
   
## 1. Pull Request
변경사항을 merge(주로 main branch에)하기 전에 리뷰를 거치기 위함이다.
* 팀원들의 동의하는 과정을 거친 뒤 대상 branch에 적용
* 줄여서 **PR**로 부르기도 한다

## Pull Request 시도하기
1. 대상 프로젝트를 local로 clone한다.
2. branch 생성과 동시에 생성한 branch로 이동한다.
```
git checkout -b [branch이름]
```
3. 변경사항을 commit한다.
4. `git push`명령어로 변경사항을 push한다   
```
git push origin [branch이름]
```
5. 이후 본인의 깃헙 저장소로 이동하면 `Compare & pull request` 버튼이 보일 것이다. 버튼을 클릭한다. 
   * 또는 `~ branches`에서 `New pull request` 클릭 
6. 메시지 작성 후 `Create pull request` 클릭

변경사항을 담당자가 확인한 뒤, main branch에 merge하게 되면 알림 메일을 받는다

## Pull Request 검토 후 처리하기
1. GitHub Repository page 내에 `Pull requests` 탭 클릭
2. 대상 Pull request 클릭하여 내용 검토
   * 의견이 있을 시 comment 달기
   * 승인할 시 `Merge pull request`
   * 반려할 시 `Close pull request`
