# @RequestBody DTO Validation



### UserController 코드 중 일부

```java
@PostMapping(value = "/signup", consumes = MediaType.APPLICATION_JSON_VALUE)
public UserSignupResponseDto signUp(@Valid @RequestBody final UserRequestDto requestDto,
                                    BindingResult bindingResult) {

    if (bindingResult.hasErrors()) {
        // 특정 field error 의 값을 담는다.
        StringBuilder sb = new StringBuilder();
        AtomicInteger i = new AtomicInteger(1);
        bindingResult.getFieldErrors()
                .forEach(fieldError -> {
                    sb.append(i.get()).append(". ").append(fieldError.getDefaultMessage()).append(" ");
                    i.getAndIncrement();
                });

        CustomException exception = new CustomException(ErrorCode.SIGNUP_INPUT_INVALID);
        exception.getErrorCode().setMessage(sb.toString()); // 필드 오류를 메시지로 설정
        throw exception;
    } else {
        return userService.signUp(requestDto);
    }
}
```



### UserRequestDto 코드 중 일부

```java
/**
* 유저 JWT 정보
*/
@NotBlank
@Expired
private String data;

/**
 * 사용자에게 입력받는 필수 정보
 */
@NotBlank
@Pattern(regexp = "^[가-힣a-zA-Z0-9]*$", message = "닉네임은 공백이나 특수문자가 들어갈 수 없습니다")
private String nickname;

@NotNull
private UserRole role;

@NotNull(message = "생년은 OOOO형식으로 입력해주세요")
private Integer birthYear;

@NotNull(message = "1~12 사이의 값을 입력해주세요")
private Integer birthMonth;

@NotBlank
private String school;
```





## Bean Validation

1. UserController에서 @Valid 어노테이션과 BindingResult를 사용해서 검증한다.
   * Dto의 `nickname` 을 예로 들면, @Pattern에서 해당 정규표현식에 만족하지 않는 문자열이 입력값으로 들어오는 경우, UserController에서 CustomException을 발생시킨다. 

2. Jwt 만료 검증을 위해, Custom Annotation을 생성하였다. 
   * `@Expired` 어노테이션을 생성해서, jwt 토큰 만료 검증



## Type Mismatch

* 하지만, Type Mismatch, 즉, 예를 들어 아래의 json 예시처럼 nickname이 `"` 로 종료되지 않은 값이 들어오는 경우 혹은 birthMonth에서 Integer 타입이 아닌 String 타입이 입력값으로 들어오는 경우, 스프링 부트는 Bean Validation을 사용해서 오류 처리를 하는 것이 아닌, `JsonMappingException` 이 발생한다. 

```json
// 타입이 일치하지 않는 예시; nickname, birthMonth
{
 "nickname" : "서울과기대21,
  "role" : "MENTEE",
  "birthYear" : 1999,
  "birthMonth" : "ㅁㄴㅇㄹ",
  ...
}
```

```java
// nickname error
Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: Illegal unquoted character ((CTRL-CHAR, code 10)): has to be escaped using backslash to be included in string value; nested exception is com.fasterxml.jackson.databind.JsonMappingException: Illegal unquoted character ((CTRL-CHAR, code 10)): has to be escaped using backslash to be included in string value<EOL> at [Source: (org.springframework.util.StreamUtils$NonClosingInputStream); line: 3, column: 38] (through reference chain: seoultech.capstone.menjil.domain.user.dto.request.UserRequestDto["nickname"])]

// birthMonth error
Resolved [org.springframework.http.converter.HttpMessageNotReadableException: JSON parse error: For input string: "ㅁㄴㅇㄹ"; nested exception is com.fasterxml.jackson.databind.JsonMappingException: For input string: "ㅁㄴㅇㄹ" (through reference chain: seoultech.capstone.menjil.domain.user.dto.request.UserRequestDto["birthMonth"])]
```

```json
// 위의 오류 발생 시 출력되는 내용
{
  "timestamp": "2023-05-29T05:43:51.972+00:00",
  "status": 400,
  "error": "Bad Request",
  "path": "/users/signup"
}
```

* 따라서 `JsonMappingException` 역시 처리해야 한다.



## JsonMappingException 

### 1. @ExceptionHandler 사용

* If you want to handle it locally for a specific controller, you can place the `@ExceptionHandler` method within that controller instead of using `@ControllerAdvice`.
* UserController 코드 내부에 아래 코드를 위치시켜서, JsonMappingException을 처리한다.

```java
@ExceptionHandler(value = {JsonMappingException.class})
public ResponseEntity<String> handleJsonMappingException(JsonMappingException e) {
    return ResponseEntity.status(HttpStatus.BAD_REQUEST)
            .body(e.getMessage());
}
```



### 2. Custom Deserializer를 사용

> [How to catch a @JsonFormat exception in spring and handle it gracefully to process the payload?](https://stackoverflow.com/questions/55137143/how-to-catch-a-jsonformat-exception-in-spring-and-handle-it-gracefully-to-proce)

```java
// CustomJsonStringDeserializer
public class CustomJsonStringDeserializer extends JsonDeserializer<String> {
    @Override
    public String deserialize(JsonParser p, DeserializationContext ctxt) throws IOException, JacksonException {
        try {
            return p.getText();
        } catch (JsonParseException e) {
            throw new CustomException(ErrorCode.NOT_A_VALID_STRING_TYPE);
        }
    }
}
```

```java
// UserRequestDto

@NotNull(message = "생년은 OOOO형식으로 입력해주세요")
private Integer birthYear;

@NotNull(message = "1~12 사이의 값을 입력해주세요")
@JsonDeserialize(using = CustomJsonIntegerDeserializer.class)
private Integer birthMonth;

@NotBlank
@JsonDeserialize(using = CustomJsonStringDeserializer.class)
private String school;
```

* 위와 같이 CustomDeserialize class를 만든 뒤, DTO에 `@JsonDeserialize` 어노테이션에 등록을 하면, **Type Mismatch** 오류가 발생 시에, CustomDeserialize class 내의 코드를 확인해서 CustomException을 반환한다. 

