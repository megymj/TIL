# @RequestParam Validation

```java
@Slf4j
@RestController
@RequiredArgsConstructor
@RequestMapping("/users")
public class UserController {

    private final UserService userService;

    /**
     * 닉네임 중복 체크
     */
    @GetMapping("/check-nickname")
    public UserAcceptDtoRes checkNicknameDuplicate(@RequestParam("nickname") String nickname) {

        return UserAcceptDtoRes
                .builder()
                .status(HttpStatus.OK)
                .message(userService.checkNicknameDuplication(nickname))
                .build();
    }
}
```

* 위 코드에서, GET 요청으로 들어오는 `nickname` 을 검증하고자 하였다.
  * null, "", " " 등과 같은 값 제외
  * 숫자만 들어오는 경우 제외
  * 그 외 등등..



## 시도 1

```java
@Validated
@GetMapping("/check-nickname")
public UserAcceptDtoRes checkNicknameDuplicate(@RequestParam("nickname")
                                               @NotBlank(message = "공백 싫어요")
                                               String nickname) {

    return UserAcceptDtoRes
            .builder()
            .status(HttpStatus.OK)
            .message(userService.checkNicknameDuplication(nickname))
            .build();
}
```

* 함수 위에 `@Validated` 를 선언하니 @Notblank 가 동작하지 않음



### 시도 1-1

```java
@GetMapping("/check-nickname")
public UserAcceptDtoRes checkNicknameDuplicate(@Validated @RequestParam("nickname")
                                               @NotBlank(message = "공백 싫어요")
                                               String nickname) {

    return UserAcceptDtoRes
            .builder()
            .status(HttpStatus.OK)
            .message(userService.checkNicknameDuplication(nickname))
            .build();
}
```

* 시도1과 결과 동일. 



## 시도 2

```java
@Validated
public class UserController {

    private final UserService userService;

    /**
     * 닉네임 중복 체크
     */
    @GetMapping("/check-nickname")
    public UserAcceptDtoRes checkNicknameDuplicate(@RequestParam("nickname")
                                                   @NotBlank(message = "공백 싫어요")      
                                                   String nickname) {

        return UserAcceptDtoRes
                .builder()
                .status(HttpStatus.OK)
                .message(userService.checkNicknameDuplication(nickname))
                .build();
    }
}
```

* class 위에 `@Validated` 를 선언하니 @Notblank 가 정상적으로 인식됨
  * Postman으로 테스트 결과, 정말 빈 값이 들어간 경우 오류를 출력하지만, "", " " 등과 같은 경우는 정상적으로 요청 처리함
  * 또한 3333, 2424 등과 같은 String type이 아닌 정수 타입이 들어와도 오류가 발생하지 않는다.



### 시도 2-1.

```java
@Validated
public class UserController {

    private final UserService userService;

    /**
     * 닉네임 중복 체크
     */
    @GetMapping("/check-nickname")
    public UserAcceptDtoRes checkNicknameDuplicate(@RequestParam("nickname")
                                                   @NotBlank(message = "공백 싫어요")
                                                   @Pattern(regexp = "^[가-힣A-Za-z0-9]*$", message = "잘못된 인풋")
                                                   String nickname) {

        return UserAcceptDtoRes
                .builder()
                .status(HttpStatus.OK)
                .message(userService.checkNicknameDuplication(nickname))
                .build();
    }
}
```

* `@Pattern` 정규표현식을 통해 검증이 추가적으로 가능하다.

  * 하지만, 이 경우 Exception을 `CustomException` 형식으로 반환하지 못하고, 스프링 부트에 내장된 Exception이 출력된다.

    ```json
    {
        "timestamp": "2023-05-14T14:36:26.824+00:00",
        "status": 500,
        "error": "Internal Server Error",
        "path": "/users/check-nickname"
    }
    ```

    

## 시도 3: BindingResult 사용

```java
@GetMapping("/check-nickname")
public UserAcceptDtoRes checkNicknameDuplicate(@RequestParam("nickname")
                                               @NotBlank(message = "nickname must not be blank")
                                               @Pattern(regexp = "^[가-힣A-Za-z0-9]*$", message = "잘못된 인풋")
                                               String nickname, BindingResult bindingResult) {


    if (bindingResult.hasErrors()) {
        throw new CustomException(ErrorCode.NICKNAME_DUPLICATED);
    }

    return UserAcceptDtoRes
            .builder()
            .status(HttpStatus.OK)
            .message(userService.checkNicknameDuplication(nickname))
            .build();
}
```

* 실행 결과-오류 메시지 출력: An Errors/BindingResult argument is expected to be declared immediately after the model attribute, the @RequestBody or the @RequestPart arguments to which they apply



## 230514 결론

@RequestParam 사용 시에는, 그냥 if 문을 사용해서 검증하기로 결정함

```java
@GetMapping("/check-nickname")
public UserAcceptDtoRes checkNicknameDuplicate(@RequestParam("nickname") String nickname) {

    String pattern1 = "^[가-힣a-zA-Z0-9]*$";    // 특수문자, 공백 모두 체크 가능

    if (nickname.replaceAll(" ", "").equals("")) { // 먼저 공백 확인
        throw new CustomException(ErrorCode.NICKNAME_CONTAINS_BLANK);
    }
    if (!Pattern.matches(pattern1, nickname)) {
        throw new CustomException(ErrorCode.NICKNAME_CONTAINS_SPECIAL_CHARACTER);
    }

    return UserAcceptDtoRes
            .builder()
            .status(HttpStatus.OK)
            .message(userService.checkNicknameDuplication(nickname))
            .build();
}
```



### 추가

* Spring에서 제공하는 `SpringUtils` 를 사용하면 공백 확인 등 `String` 을 다루는 여러 가지 메서드를 제공하지만, 굳이 사용할 필요가 없다고 생각해서 사용하지 않았음

