# Emulator SD카드에 이미지 넣기

갤러리에서 이미지를 받아오기 위해, Emulator SD카드에 이미지를 넣는 절차.



### 절차

1. Android Studio에서 Emulator 실행시킴
2. View > Tool Windows > Device File Manager 로 들어감
   * storage > emulated > 0 > pictures 내에 이미지 파일 넣기
3. storage 디렉터리에서 마우스 우클릭을 한 뒤, Synchronize 클릭
4. 1~3번을 수행해도, emulator에 반영되지 않는다. 따라서, 몇 번 Emulator를 켰다 종료시켰다를 반복해야 함



### 주의사항

* 대상 emulator를 "Wipe Data"로 지우게 되면, 파일이 전부 날아간다. 
* 대상 emulator를 "Cold Boot Now"를 수행하면, 이미지 파일들의 size가 모두 0 Byte가 된다.

따라서 Emulator SD카드에 이미지를 넣었다면, 위 두 과정을 시도하지 말 것.