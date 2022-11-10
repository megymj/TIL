# etc

* 기타 git 사용 시 내가 사용한 내역들



###  1. git checkout

* 원격 branch인 origin/feature를 추적하는 새로운 branch를 생성하는 방법

```bash
# 1. 
git checkout --track origin/feature
or 
git checkout -t origin/feature

# 2.
git checkout --track -b feature origin/feature
```





### 2. 협업 전략

* git rebase 사용 시에는 되도록 branch를 새로 생성한 뒤, 해당 branch와 rebase를 한다.