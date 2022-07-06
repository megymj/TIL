reference link: [tistory1](https://songjang.tistory.com/91), [tistory2](https://somjang.tistory.com/entry/macOS%EC%97%90-nvm%EC%84%A4%EC%B9%98%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-feat-brew)

# Mac] Install node

``````bash
# 1. install nodejs
brew install nvm

vi ~/.zshrc
export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && . "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && . "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"

저장 후
sourc ~/.zshrc
nvm --version # 설치 확인

nvm install 10.10.0 # node.js 10.10.0 version 설치
node -v # check install

npm install -g nodemon
npm start


# 2. install Express
npm install express-generator -g

express ExpressTest1

# 이후 아래 3가지 명령어를 입력하면 된다.
cd ExpressTest1
npm install
DEBUG=ExpressTest1:* npm start

# run 확인
localhost:3000
 
```
