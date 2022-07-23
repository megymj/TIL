# Screen

* screen이란 Linux에서 독립적으로 동작하는 가상 터미널을 띄워주는 것을 의미한다. 즉, 백그라운드로 동작하는 가상 터미널이다. 

* 장점: 스크린에서 명령어를 실행시키고 터미널을 꺼도, 명령어가 백그라운드로 계속 돌아간다. 명령어를 실행시킨 뒤 터미널을 종료하고, 나중에 screen 명령을 이용해서 다시 접속하면 해당 터미널 그대로 작업을 이어갈 수 있다.

현재 ubuntu 서버에서 'screen' 명령어가 작동되는 것을 보아, 서버에 screen이 설치되어 있는 것 같다.

혹은, `$ screen --version` 명령어를 통해 version 확인 및 설치 여부를 조회할 수 있다.

(설치되어 있지 않다면 'screen'명령을 쳤을 때 Command not found 라고 뜬다고 함)



## Command

### 1. Screen 사용 명령어

```bash
# 기본 사용법
$ screen  # screen 진입(이름은 무작위로 생성됨)
  
# screen 만들기
$ screen -S screen_name   # screen_name 부분에 명칭 입력하면 됨
  
# screen 리스트 확인하기
$ screen -list 
$ screen -ls
  
# screen 다시 들어가기: detached 되어있는 screen에 attach(접속)할 수 있다.
$ screen -r screen_name   # screen_name 부분에 명칭 입력하면 됨
  
# screen 삭제
$ screen -X -S screen_name kill
  
# screen 종료
# 해당 screen에 접속한 이후 exit을 적어 주면 해당 스크린을 종료하고 터미널로 돌아옴
```



### 2. screen 세션 접속 후 명령어

```bash
 # screen 실행 후 명령어는 Ctrl + a로 시작함
$ ctrl + a + d    # 세션을 종료하지 않고 현재 작업을 유지하면서 screen 세션에서 빠져 나옴
   # screen attach 이후 screen detach
```





## 전체 디스크에 대한 용량 확인

```bash
df -h # 보기 좋게 출력
```

