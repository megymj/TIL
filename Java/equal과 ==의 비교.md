# equal과 ==의 비교

`equal` : 대상의 내용을 비교

`==` : 대상의 주소(reference)값을 비교

객체의 예를 들면,

```java
Circle A = new Circle(3);
Circle B = new Circle(3);
```

에서 `A != B` 이고 `A is equal to B` 이다. 

* 단, Object class의 equals 메소드는, String class의 equals 메소드와는 달리 `==`연산자로 두 객체를 비교하기 때문에 위에서 Circle 객체의 값의 일치 여부를 확인하기 위해서는 equals 메소드를 Overriding 해줘야 한다. 



### same

Junit assertThat에서 사용하는 `isSameAs`, `isNotSameAs`는 `==` 의 의미이다.