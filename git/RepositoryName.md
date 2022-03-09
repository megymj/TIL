## GitHub에서 Repository 이름 및 url 변경

* 기존에 HackerRank 사이트에서 SQL 문제들을 풀며, 푼 내용들을 기록하기 위해 'HackerRank_Solution'이라는 GitHub repository를 만들었는데, 앞으로 SQL을 계속 공부할 예정이라 이 repository를 이용하기로 했다. 
* 따라서, repository의 명칭을 `HackerRank_Solution -> SQL_Study`로 변경하려고 한다.

### 1. 해당 레포지토리에 들어가서, setting 버튼을 누르고, Rename을 통해 새 명칭을 만든다.
   ![rename](https://user-images.githubusercontent.com/80478750/157265662-a5b27889-34e5-49f9-9e08-bbac431c12cf.PNG)

### 2. 명칭을 변경한 뒤, 로컬 저장소의 원격 url을 변경한다.
   * repository의 명칭을 변경하면 url도 변경되기 때문에, 로컬 저장소에도 변경사항을 꼭 적용해야 한다.
   * `git remote set-url origin [변경된 repository url]`: 이 명령어를 실행하여 로컬 저장소에 바뀐 url을 적용한다.
   * `git remote -v`: 제대로 적용이 되었는지 확인한다.
![2](https://user-images.githubusercontent.com/80478750/157266795-3c1c03bf-4f45-401b-a413-f9e93a132948.PNG)

### 여기서 생기는 두 가지 의문점
1. 1번에서 repository의 명칭을 변경한 뒤, 만약 기존의 로컬 저장소에서 새로 추가된 내용이 없다면, 기존의 로컬 저장소를 삭제하고 다시 git clone해서 받아오면 되지 않나?
2. 위의 1,2번 작업 진행 후, 로컬 저장소(디렉토리)의 명칭은 SQL_Study가 아닌 HackerRank_Solution으로 남아있는데, 원래 변경되지 않는 것인가?
2-1. 디렉토리명은 HackerRank_Solution으로 남아 있으나, commit, push, pull 등은 무리없이 GitHub SQL_Study로 잘 수행이 되는 것으로 봐서, 디렉토리명은 별로 중요하지 않은가? .git폴더만 잘 유지하면 되는 것인가?

언젠가 궁금증이 해결된다면, 다시 이 글에 답변을 남길 예정.
