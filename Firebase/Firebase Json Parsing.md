# Json Parsing



### 1. Android

Firebase Realtime Database에 저장된 data를 안드로이드로 읽어와서 출력할 때, 줄 바꿈(개행)이 적용되도록

* Firebase에는 줄 바꿈할 위치에 "\n"을 넣는다.

```json
// json 코드 중 일부
...
"content": "성수 디올을 구경하고 성수동 카페를 찾다가 괜찮아보여서 방문한 할아버지 공장!\n 뷰로 시작해서 뷰로 끝나는 카페!! 특히 가을에 너무 예쁜 곳입니다.\n 다음엔 음료 이외에 디저트도 먹을거에요!",
...
```



```java
// Android code

@Override
public void onDataChange(DataSnapshot dataSnapshot) {
  for (DataSnapshot postSnapshot: dataSnapshot.getChildren()) {

    String Content = fbPost.getContent().replace("\\n", "\n");  

    textView.setText(Content)

  }
}
```

