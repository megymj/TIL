## null
파이썬 등의 언어에서는 '비어있음'을 None으로 표현하고, 자바 등의 언어는 null로 표현한다.  
단, null은 참조형 변수(Reference Type)만 가질 수 있는 기본 값이다.  
기본형 타입 중 boolean은 false, int는 0을 기본 값으로 가지는 것과 동일한 의미이다. 

```java
Person p1 = null;
System.out.println(p1);
// 출력결과 : null

System.out.println(p1.getName());
// 출력결과 : Exception in thread "main" java.lang.NullPointerException
