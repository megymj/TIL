# SourceTree] Invalid username or password. error 해결 방법

#### MacOS 기준

간혹 GitHub의 Token이 만료 기간까지 한참 남았음에도 불구하고, 소스트리로 pull or push 등의 명령어를 사용할 때, 

> invalid username or password. 

오류가 발생하는 경우가 종종 있었다.

따라서 이번 기회에 해당 오류를 해결하는 방법을 정리하려고 한다.



### 방법: 기존 github 계정 초기화 및 다시 token 생성

1. SourceTree와 MacOS keychain에 등록된 계정을 완전히 제거한다.

   * SourceTree > settings > Advanced 에 등록된 계정을 제거한다.
   * Keychain Access > 검색창에 sourcetree를 입력한 뒤, "github.com keychain access ..." 로 나오는 계정을 삭제한다.

2. GitHub 사이트에서 기존 token을 삭제한 뒤, 새로운 token을 재발급한다.

3. 다시 소스트리로 가서, pull / push / fetch 중 아무 작업이나 시도하면, 다시 아이디와 패스워드(token)을 입력하라는 창이 뜬다. 이때 변경된 token을 입력한다.

4. cmd 창에서도 변경 내용을 적용한다.

   ```bash
   $ git config --global user.name "username"
   $ git config --global user.email "email@email.com"
   ```

5. 이후 cmd 창에서 pull / push / fetch 중 아무 작업이나 시도하면, 다시 아이디와 패스워드(token)을 입력하라는 창이 뜬다. 이때 변경된 token을 입력한다.



