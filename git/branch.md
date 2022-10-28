# branch

### 1. git rebase로 branch 합치기

* 아래 사진에서, develop_kbs branch와 develop_moojun branch가 있다. 
* develop_moojun branch 에 develop_kbs branch 내용을 덮어쓰려고 함(충돌을 무시하고 강제로)
* ***하지만 branch 간 git push --force 같은 명령어는 찾지 못해서, 결국 rebase를 통해 합치고자 함***


<img width="846" alt="branch1" src="https://user-images.githubusercontent.com/80478750/198573971-17a36d78-55ce-4175-adcb-74048c568ce0.png">



```bash
develop_kbs] git rebase develop_moojun

# 이후 충돌 날 때 마다 아래 명령어들을 사용함
develop_kbs] git add .
develop_kbs] git rebase --continue

# rebase가 완료되면 자동으로 develop_mojun branch로 이동되는데, 이 때 fast-forward 를 위해 merge
develop_moojun] git merge develop_kbs
```

<br>

#### 하지만 위에 방법처럼 수행한 결과, 내가 뭔가를 잘못 commit 내역이 복사가 되어서 origin에 push됨. 

###### 이 상황을 해결하고자 한다. 아래 사진에서, 빨간색 부분의 커밋들을 없애는 것이 목표. 

<img width="837" alt="branch2" src="https://user-images.githubusercontent.com/80478750/198585427-291dcb67-1b1e-444b-abef-cc3f3700481e.png">


