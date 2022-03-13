> 참고: [stack overflow](https://stackoverflow.com/questions/36841461/error-android-emulator-gets-killed). 하지만 별 도움이 되지는 않았다.
## The emulator process for avd has terminated(error)

### AVD emulator 실행 오류
![terminated](https://user-images.githubusercontent.com/80478750/158062286-53fbf2b0-1479-49b6-ba58-dc38c145842b.PNG)

### 해결 방법
#### 1. settings > android SDK > install 'Intel x86 Emulator Accelerator(HAXM installer)
* 결과: 설치하려고 하니 오류가 발생해서 설치가 되지 않았다.

#### 2. 새 API device를 생성한 뒤, 해당 device로 run. 기존에 오류 발생하던 device 삭제
* 이 방법으로 문제를 해결했다.


  
#### ※ 최후의 수단: android studio 재설치
* 제어판 > 프로그램 추가&제거 > Android Studio 삭제

* 이후 아래 목록의 디렉토리 및 파일 모두 삭제

  * `C:\Program Files\Android\Android Studio`

  * `C:\Users\[사용자 계정 폴더]\.android`

  * `C:\Users\[사용자 계정 폴더]\.AndroidStudio<version>`
  
  * `C:\Users\[사용자 계정 폴더]\AppData\Local\Android\Sdk`

* **주의: 삭제하기 전에 프로젝트 파일들 백업할 것!**
