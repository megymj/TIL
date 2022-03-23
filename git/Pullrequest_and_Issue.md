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

`보통 branch는 작업 단위로 생성하므로, 하나의 issue를 처리하면서 사용된 local의 branch는 main branch에 merge된 이후 제거하는 것이 좋다`

## Pull Request 검토 후 처리하기
1. GitHub Repository page 내에 `Pull requests` 탭 클릭
2. 대상 Pull request 클릭하여 내용 검토
   * 의견이 있을 시 comment 달기
   * 승인할 시 `Merge pull request`
   * 반려할 시 `Close pull request`



## 2. Issue
Git 공식 문서를 보면,
* Use issues to track ideas, enhancements, tasks or bugs for work on GitHub
* You can collect user feedback, report software bugs, and organize tasks you'd like to accomplish with issues in a repository
* You can link a pull request to an issue to show that a fix is in progress and to automatically close the issue when someone merges the pull request   

라고 설명되어 있다.

### Issue 예시
* [elasticsearch](https://github.com/elastic/elasticsearch/issues)
* [flutter](https://github.com/flutter/flutter/issues)

## Issue 작성하기
1. GitHub Repository page 내에 `Issues` 탭 클릭
2. 이슈 작성
   * 필요시 label, milestone, asignee 지정
   * label: 해시태그 같은 것이다. 기존의 label 사용 혹은 새로 label을 만들 수 있다
   * asignee: 해당 이슈를 맡을 책임자를 지정한다
   * milestone: issue들을 그룹화한다

3. 이슈 확인 후 처리
   * 의견 남기기(comment)
   * 관련 개발 착수
   * 해결한 뒤: `Close issue`
