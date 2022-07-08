# JavaScript] ReferenceError: document is not defined

vscode에서 .js 파일 하나를 생성한 다음, 
```javascript
Document.write(30);
```
을 입력하고 실행하니 
`ReferenceError: document is not defined` error가 발생하였다.

이에 대한 좋은 답변이 있어서 해당 내용을 기록하였다( [참고 링크](https://okky.kr/article/532280?note=1583450) )
* JavaScript는 처음 언어가 만들어질 때 브라우저 안에 존재하였다. 따라서 JS는 자신을 둘러싼 브라우저 환경 안에서 코드가 해석되고 실행된다. window, document 등과 같은 이름을 가진 객체는 브라우저 환경을 의미한다. 
그런데 JS를 브라우저 밖으로 끄집어내서 c, c++, java 등과 같이 범용 언어처럼 사용하는게 NodeJS이다. 따라서, 내가 짠 코드가 JS지만 실행되는 곳이 브라우저가 아니다.

* 만약 위의 코드를 .html 파일 내에 아래와 같은 방식으로 적으면 정상적으로 잘 동작한다.
```html
<script>
  Document.write(30);
</script>
```



