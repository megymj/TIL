# YearMonth



Dto 객체로부터 `yyyy-MM` 의 날짜 데이터를 받기 위해서 처음에는 `LocalDate` 를 사용하였음

```java
@NotNull
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM", timezone = "Asia/Seoul")
@DateTimeFormat(pattern = "yyyy-MM")
private LocalDate birthDate;
```

이후 json 객체에서 요청 타입에 아래와 같은 형식으로 요청을 전달하였으나, deserialize 과정에서 실패함

> Cannot deserialize value of type `java.time.LocalDate` from String "2018.12" 

```json
{
  "birthDate" : "2018.12",
  ...
}
```



<br>

`yyyy-MM-dd` 형식은 올바르게 역직렬화가 된다.

```java
@NotNull
@JsonFormat(shape = JsonFormat.Shape.STRING, pattern = "yyyy-MM-dd", timezone = "Asia/Seoul")
@DateTimeFormat(pattern = "yyyy-MM-dd")
private LocalDate birthDate;
```

```json
{
  "birthDate" : "2018.12.01",
  ...
}
```



<br>

## LocalDate가 아닌 YearMonth 사용

하지만 JPA Hibernate를 사용해서 `yyyy-MM` 형식을 YearMonth 타입으로 넣는 경우, 

birth_date에 값이 `2018-12` 가 아닌, `0xACED00057372000D6A6176612E74696D652E536572955D84BA1B2248B20C0000787077060C000007E20B78` 와 같은 값이 입력이 되는 것을 확인할 수 있다. 

이를 위해서는, https://vladmihalcea.com/java-yearmonth-jpa-hibernate/ 링크처럼 따로 parsing class를 만든 뒤, birthDate field에 주입을 해야 하는 것으로 보여서, 그냥 String Type으로 입력 받기로 하였으며, Dto에서는 정규 표현식을 사용하여 OOOO-OO 형식에 맞게 오는지만 검증하기로 결정하였다.



