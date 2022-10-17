# .gitignore 

> reference link: [.gitignore not ignoring .idea path](https://stackoverflow.com/questions/32384473/gitignore-not-ignoring-idea-path)



## 1. .gitignore only ignores newly added(untracked) files

```bash
# Intellij 의 경우, .idea 폴더가 처음에 .gitignore에 등록되어 있지 않다. 
# 이미 Github에 .idea/ 폴더가 올라간 경우, 지우고 싶을 때 아래 명령어들을 사용

git rm -r --cached .idea/
git add .gitignore
git commit -m "Removed .idea files"
```

