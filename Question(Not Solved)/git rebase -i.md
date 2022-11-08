# git author와 commit이 다른 경우



### 0. 문제설명

* Triplanner 팀 프로젝트 중 발생한 상황. `develop_kbs` branch의 내용을 `develop_moojun` branch로 rebase하기 위해, `develop_kbs` 에서 `develop_kbs_test` branch를 생성한 다음, 이 branch로 `develop_moojun` branch에 rebase하였으며, rebase 이후 `develop_kbs_test` branch는 정상적으로 제거하였다. 

* rebase 한 내역을 원격의 `develop_moojun` 으로 push 한 이후 원격의 커밋 내역을 보니, 아래와 같았다. 

* 여기서 두 가지의 문제가 발생한 것을 확인하였다.

  1. 총 12개의 커밋에 대해 **ninanoray authored and Moojun committed** 라는 문구가 발생하였다. 이 12개의 커밋의 경우 **ninanoray** 가 작성한 커밋이기 때문에 원래는 **ninanory committed 27 days ago** 와 같은 형식으로 나와야 하는데, Moojun이 committed 되었다는 내용이 왜 포함되었는지 모르겠다.

  2. 아래 커밋 내역 중 ""장소 선택 단계에서..." ~ "plannerSelect에서 장소를 ..." 까지 5개의 커밋 내역은 기존에 나에게도 있던 커밋 내역인데, rebase를 하니 다시 반영된 것을 알 수 있다. 

  * 첫 번째 사진은 1번, 두 번째 사진은 2번과 관련된 내용이다.

<img width="899" alt="1" src="https://user-images.githubusercontent.com/80478750/200610272-150bcf31-e7cb-4694-a469-7073e6a80dd6.png">

<img width="428" alt="2" src="https://user-images.githubusercontent.com/80478750/200611303-5f741491-6384-4b4a-b120-d41129363f27.png">





### 1번 해결 방안

> 참고 링크
>
> https://velog.io/@shin6949/Git-Commit-기록-정리하기-git-rebase
>
> https://madplay.github.io/post/change-git-author-name
>
> https://otrodevym.tistory.com/entry/git-commit-한-author-변경작성자-변경-방법
>
> https://madplay.github.io/post/change-git-author-name



#### 1-1. 수정할 커밋 고르기

나의 경우 GitHub를 참고하면 총 12개의 커밋을 수정해야 한다.

> git rebase -i HEAD~12



이후 아래 창에서 "Pick"을 모두 "e" 혹은 "edit" 으로 변경한 뒤, "wq"로 저장하고 나온다.

<img width="805" alt="3" src="https://user-images.githubusercontent.com/80478750/200613822-335873c9-b489-4bfc-ae00-c91140b220a0.png">



#### 1-2. Author 정보 수정하기

아래 명령어를 통해 Author의 정보를 변경할 수 있다. commit message가 뜨면 역시 "wq"로 저장하고 나온다.

> git commit --amend --author="Github ID  <email>"



#### 1-3. git rebase --continue

2번에서 정보를 수정한 뒤, 아래 명령어를 통해 다음 commit으로 넘어간다. 아래 사진은 예시이다. 이 과정을 반복한다.

> git rebase --continue

<img width="815" alt="4" src="https://user-images.githubusercontent.com/80478750/200614343-7efee588-9c69-444a-a57a-6ed9d356ff00.png">





#### 1-4. 수정이 모두 완료되었다면, 원격에 강제로 푸쉬한다.

test를 위해, develop_moojun branch가 아닌 moojun-test2 branch를 만들고 실험한 결과이다.

> git push -f origin moojun-test2 	
>
> or 
>
> git push -f origin +moojun-test2



<img width="566" alt="5" src="https://user-images.githubusercontent.com/80478750/200614957-7bdb9c5c-ba2b-45b4-a3ac-9aa7849cd909.png">





### 2번 해결 방안

1번을 완료한 이후, 여러 블로그를 검색하다 보니 아래의 내용을 발견했다.



#### 2-1. commit 시간 반영

GitHub에서는 commit date를 받아 오는데, git rebase를 수행하면 commit date가 변경되기 때문에 아래와 같은 현상이 발생하는 것으로 보인다고 한다. 아래 명령어를 통해 확인 가능하다.

* AuthorDate와 CommitDate가 불일치하는 것을 확인할 수 있다. 

> git log --pretty=fuller
>
> 



<img width="815" alt="6" src="https://user-images.githubusercontent.com/80478750/200614966-5888be8c-7efc-46fa-a2af-beb35f4ada88.png">



아래 명령어로 해당 문제를 해결한다.

> git filter-branch --env-filter 'export GIT_COMMITTER_DATE="$GIT_AUTHOR_DATE"'



#### 2-2. 원격에 강제로 푸쉬한다.

test를 위해, develop_moojun branch가 아닌 moojun-test2 branch를 만들고 실험한 결과이다.

이렇게 하니, 1-4번에 있던 커밋 내역에 비해 조금 더 단순해 졌으며, 0번 문제설명에서 얘기했던 5개의 커밋이 겹치던 것이 최신 커밋에서는 사라진 것을 볼 수 있다. 

> git push -f origin moojun-test2 	
>
> or 
>
> git push -f origin +moojun-test2

<img width="401" alt="7" src="https://user-images.githubusercontent.com/80478750/200617332-03d9e32d-00cc-42c9-8c8f-f38742528794.png">









### 3. 해결 과제

* 1번에서 author를 Moojun으로 변경하여 커밋을 깔끔하게 만들었으나, 이는 내가 의도한 것이 아니었다.  이 12개의 커밋은 모두 **ninanoray** 가 작성한 커밋이기 때문에 **ninanory committed 27 days ago** 와 같이 작성되어야 하지만, author를 변경하였기 때문에 **Moojun committed 1 minute ago** 와 같이 주체가 변경되어 버렸다. 

  * 이를 어떻게 해결할 것인가? 애초에 rebase를 처음 할 때 제대로 했다고 생각하는데 왜 author와 committ가 겹쳐지는 현상이 발생하였는지는 아직도 원인을 잘 모르겠다.

  



