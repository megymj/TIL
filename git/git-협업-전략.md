# git 협업 방법



### 1. 두 개의 branch간에 서로 협업을 하는 경우



#### git rebase 방식에서의 문제점

<img width="923" alt="rebase" src="https://user-images.githubusercontent.com/80478750/201522060-bb2dcaed-3d34-4c9f-8c6b-fba0a6cb1957.png">

* ddev-feat1 branch와 dev-feat2 branch에서 서로 다른 작업을 진행하고 있다. 이 때, dev-feat1 branch에서 dev-feat2 branch의 작업 내용을 받아오기 위해, `git merge` 가 아닌 `git rebase` 방식을 사용하였다. 
  * 그 결과, 정상적으로 dev-feat2의 두 개의 커밋이 dev-feat1으로 반영되었다.


* 하지만, 이후 dev-feat2에서 dev-feat1의 `commit-f1(1)` 과 `commit-f1(2)` 의 커밋 내역을 받기 위해 `git rebase` 를 사용하면, `commit-f1(1)` 과 `commit-f1(2)` 외에도 **commit-f2(1)** 과 **commit-f2(2)** 가 rebase로 다시 들어온다. 즉, dev-feat2의 경우, rebase 이후에는 **commit-f2(1)** 과 **commit-f2(2)** 가 두번 남는 것이다.
* 이 경우 dev-feat2 branch의 경우 같은 내용의 커밋이 두 번 중복되어서, 커밋이 매우 지저분해지는 문제가 발생한다.





#### develop branch에 PR 날리는 방식

git-flow 방식 중 일부를 참고함

<img width="1007" alt="PR" src="https://user-images.githubusercontent.com/80478750/201522058-4d89ce8b-be2a-4701-8e03-2bb9598b7654.png">

1. develop branch에서, dev-feat1, dev-feat2 branch를 만들고, 각각 checkout(switch)해서 작업을 수행한다.
2. dev-feat1 branch에서 작업을 진행한 뒤, 원격으로 `push` 한 이후 `Pull Request` 를 develop branch로 생성한다. 이 때, develop branch에서 변경사항이 없었기 때문에 Conflict가 발생하지 않고 정상적으로 merge가 가능하다는 내용이 뜰 것이다.
3. 이후 dev-feat2에서 작성한 커밋 내역을 원격으로 `push` 한 뒤, 이후 develop branch로 `Pull Request` 를 생성하면 높은 확률로 Conflict가 발생할 수 있다. 왜냐하면 dev-feat1 branch에서 한 작업 내역과 dev-feat2 branch에서 한 작업 내역이 겹칠 수 있기 때문이다. 
   * 아래 사진은 PR시 conflicts가 발생한 예시이다.

<img width="983" alt="pr2" src="https://user-images.githubusercontent.com/80478750/201523381-2854810e-f975-4860-9eaa-8d8ee9f60e25.png">



4. Conflicts를 해결한다. 방법은 두 가지가 있으며, 위의 사진에서 보이는 것처럼 `web editor` 를 사용하거나 `command line` 을 사용하는 것이다.

   * `web editor` 를 클릭하면 GitHub의 web editor에서 변경할 수 있다. 

   * `command line` 을 클릭하면 CLI를 사용해서 conflicts를 해결하는 방법을 보여준다.

     ```bash
     # Step 1: Clone the repository or update your local repository with the latest changes.
     git pull origin develop
     
     # Step 2: Switch to the head branch of the pull request.
     git checkout dev-mj
     
     # Step 3: Merge the base branch into the head branch.
     git merge develop
     
     # Step 4: Fix the conflicts and commit the result.
     See Resolving a merge conflict using the command line for step-by-step instructions on resolving merge conflicts.
     
     # Step 5: Push the changes.
     git push -u origin dev-mj
     ```

      

5. ~~만약 `command line` 을 사용해서 로컬에서 conflicts를 해결하는 경우에는, 기존의 PR을 닫고 충돌을 해결한 뒤 다시 PR을 열어야 하는 것으로 보인다.~~   

   > command line을 사용해서 로컬에서 작업을 한 이후, 충돌 해결 내역을 커밋한 뒤 원격으로 Push하니, PR 창이 재로드 되면서 성공적으로 merge가 가능하다는 창이 뜨는 것을 확인했다. 



최종적으로는 아래 사진과 같이, dev-mj와 dev-kb의 커밋 내역과, 충돌을 해결한 커밋("두 개의 작업 모두 반영")이 모두 develop branch에 잘 반영된 것을 확인할 수 있다.

<img width="624" alt="commit" src="https://user-images.githubusercontent.com/80478750/201523997-8d0611f5-b4a8-4b08-81b7-34418a6966ee.png">







