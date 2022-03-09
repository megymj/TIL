## 기본형(Primitive Type) vs 참조형(Reference Type)
기본형 : int, boolean, char, double 등 <br>
참조형 : String, int[], Person 등

### 기본형
기본형의 경우는, 변수가 값 자체를 보관한다고 생각하면 된다.
```java
int a = 3;
int b = a;

System.out.println(a);  // 3 출력
System.out.println(b);  // 3 출력

a = 4;
System.out.println(a);  // 4 출력
System.out.println(b);  // 3 출력

b = 7;
System.out.println(a);  // 4 출력
System.out.println(b);  // 7 출력

```


### 참조형(배열도 포함)
참조형의 경우에는 변수가 값 자체를 보관하는 게 아니라, 변수가 값을 '가리킨다'고 생각하면 된다.  
즉, 실제 값은 메모리의 어딘가에 저장되어 있고, 변수는 그 영역을 가리키는 역할인 셈이다.
```java
Person p1, p2;
p1 = new Person("김민준", 28);

p2 = p1;
p2.setName("이남준");

System.out.println(p1.getName()); // 이남준 출력
System.out.println(p2.getName()); // 이남준 출력

```
