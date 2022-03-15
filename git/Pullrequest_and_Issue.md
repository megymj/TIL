# GitHub의 Pull Request와 Issue
GitHub에서 자주 사용되는 Pull Request(PR)과 Issue에 대해 알아본다
   
## 1. Pull Request
변경사항을 merge(주로 main branch에)하기 전에 리뷰를 거치기 위함이다.
* 팀원들의 동의하는 과정을 거친 뒤 대상 branch에 적용
* 줄여서 **PR**로 부르기도 한다

## Pull Request 시도하기
1. 대상 프로젝트를 local로 clone한다.
2. `git checkout -b [branch이름]` 명령어를 통해, branch 생성과 동시에 생성한 branch로 이동한다.
3. 변경사항을 commit한다.
4. `git push`명령어로 변경사항을 push한다   
```
git push origin [branch이름]
```

