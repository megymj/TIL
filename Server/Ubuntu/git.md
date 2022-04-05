# Linux-Ubuntu에서 git 사용해보기

## 1. Ubuntu에 git 환경 구축하기
1. 패키지 리스트 업데이트
```
sudo apt-get install git
```
 
2. git 설치
```
sudo apt install git
```

3. git 설치 확인
```
git --version
```

4. git에 push할 때 올라갈 내 정보 입력하기
   * 다수의 경우, `git config --global`을 사용해서 등록하라고 하는데, 나의 경우 해당 프로젝트에는 다른 name/email을 등록하고 싶어서, stackoverflow를 참고함
   * [stackoverflow: 3 levels of git config](https://stackoverflow.com/questions/8801729/is-it-possible-to-have-different-git-configuration-for-different-projects)
   * 나는 5번에서 git clone 한 이후, 7번에서 user.name, user.email을 등록함

5. git clone 하기  
   * clone할 때, Username과 Password를 입력해야 하는데, Password에는 github token을 입력해야 정상적으로 실행됨
   ![image](https://user-images.githubusercontent.com/80478750/161666948-7330bd37-50ef-433a-98a5-0b9b362aaf9d.png)

6. vi로 파일 수정 후 `git status`로 변경 사항 확인 가능

7. `git add .` -> `git commit -m "커밋 메시지 작성"`
   * 4번에서 내 정보를 입력하지 않으니, 이후 commit을 시도할 때 user.name과 user.email을 등록하라는 경고 문구가 뜬다
   ![image](https://user-images.githubusercontent.com/80478750/161668053-c54b4482-14a0-4967-9593-594ecbdfe9dd.png)
   * 여기서 `git config`로 user.name과 user.email을 등록한 뒤 commit을 시도하니, 위 사진과 동일한 에러를 발생시킨다. 
   결국 `git config --global`을 사용해서 등록해야 하는 것 같다
   * `git config --list` 명령으로 위에서 등록한 user.name과 user.email 확인
   * `git config --unset user.name "user name"` 과 `git config --unset user.address "user email"` 로 데이터 지움
   * `git config --global`로 user.name과 user.email을 등록한 뒤 commit을 시도하니, 정상적으로 commit이 수행된다. 

8. 원격(GitHub)으로 push
```bash
git push
or 
git push -u origin main

결과: fatal: unable to access 'https://github.com/[레포지토리명].git': The requested URL returned error: 403
```
이후 검색을 해서 github token 사용, git set-url 등의 방법을 사용하였으나 실패하여, 해당 저장소를 fork하여 사용하기로 함.




