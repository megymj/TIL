# git] GitHub에 올라간 commit 되돌리기

> 참고 링크: [참고1](https://xtring-dev.tistory.com/entry/Git-이미-commit한-메세지-수정하기-바로-이전그-전리모트-commit), [참고2](https://jw910911.tistory.com/77)

이미 GitHub에 commit된 메시지 수정하기



### 1. 마지막 commit 메시지 수정하기

`--amend` 옵션을 사용하여 마지막에 입력한 commit 메시지 수정 가능

```bash
git commit --amend -m "commit message"

# 또는 commit message를 바로 입력하지 않고 vi 창에서 수정하는 방법
git commit --amend
```





### 2. 이전의 commit 메시지 수정하기

마지막 커밋이 아닌, 그 이전에 작성했던 커밋 메시지를 수정하기 위해서는 절차가 조금 더 복잡하다.

commit 메시지를 수정하기 위해서는 `rebase` 명령을 사용한다. 왜냐하면 실제로는 해당 commit으로 이동하고 commit 메시지를 수정하고 해당 branch에 다시 merge 하는 방식이기 때문이다. `rebase`를 통해서 merge commit이 생성되는 것을 막고 깔끔하게 commit 메시지 자체만 남길 수 있다.



#### 방법은 아래와 같다

* 아래 커밋 내역 중, "커밋 메시지 1번째" 를 수정하고자 한다. 

<img width="401" alt="git-test" src="https://user-images.githubusercontent.com/80478750/205190615-1e9fbaa6-d93b-4245-b455-325c700bedab.png">



터미널에 아래와 같이 입력한다. 

> git rebase -i HEAD~3

HEAD~3은 최근 커밋 메시지 중 3개만 불러온다는 의미이다. 

입력하면 아래와 같이 뜨는데, 내가 변경할 것은 "커밋 메시지 1번째" 이므로 앞의 `pick` -> `reword` 로 수정 후 저장한다.

<img width="676" alt="rebase1" src="https://user-images.githubusercontent.com/80478750/205206944-c27473f9-1f01-43a8-aa7f-76dc76831054.png">



reword로 수정 후 저장하면 커밋 메세지를 변경할 수 있는 창이 생기며, 해당 창에서 커밋 메시지를 변경하고 저장한다.



### 3. GitHub에 커밋 내역 덮어쓰기

* 협업 시에는 이 커맨드를 쓰는 것은 주의가 필요해 보인다. 

```bash
git push --force [branch name]
```





### + 알파

* 아래 사진에서, 커밋 메시지를 작성한 시간 순서대로 커밋이 배열되어 있는데(1번째 -> 2번째 -> 3번재), 2-3번에서는 1번째의 커밋 내용을 변경한 뒤, 강제로 원격에 push 하였다. 이 상황에서, 1번째 내용만 변경되고 2, 3번째 내용은 변경되지 않았는데, 커밋 시간대는 어떻게 조정될까?

<img width="401" alt="git-test" src="https://user-images.githubusercontent.com/80478750/205190615-1e9fbaa6-d93b-4245-b455-325c700bedab.png">



결과는 아래와 같다. 1번째 메시지를 수정하니, 2, 3번째 메시지도 자동으로 1번째 커밋 이후의 시간대로 변경되는 것을 확인하였다. 

<img width="440" alt="Screenshot 2022-12-02 at 12 23 33 PM" src="https://user-images.githubusercontent.com/80478750/205209449-4bd51533-c33d-4dfd-9d6e-ea1a523fceca.png">







