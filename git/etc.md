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





### 3. 소스트리에서 파일 이름 변경

* 첫 번째 이미지는 파일 이름을 aaa.yaml -> a.yaml로 변경한 뒤 소스트리 창을 연 결과이며, 두 번째 이미지는 마찬가지로 파일 이름을 aaa.yaml -> a.yaml로 변경한 뒤 CLI 창에서 `git add .` , `git status` 를 입력한 내역이다.
* 동일한 작업을 수행하였으나, CLI 창에서는 파일 이름이 변경된 것으로(renamed) 잘 인식하는 것에 비해, 소스트리에서는 renamed로 제대로 인식을 하지 못하는 것을 볼 수 있다. 

<img width="844" alt="Screenshot 2022-11-11 at 4 30 20 PM" src="https://user-images.githubusercontent.com/80478750/201289135-d8ca1577-57e0-4b58-83f3-c1943d20c1a2.png">

<img width="642" alt="Screenshot 2022-11-11 at 4 27 01 PM" src="https://user-images.githubusercontent.com/80478750/201288653-8eaf3b67-c007-4700-bd92-d809fcb6d3a5.png">



> 결론: git을 사용하는 프로젝트에서 파일명을 변경할 때는 **소스트리가 아닌 CLI 창에서 mv** 명령을 사용해서 파일 이름이 변경되었다는 것을 명시해 줄 필요가 있는 것으로 보인다.

```bash
# example
git mv aaa.yaml a.yaml
```

