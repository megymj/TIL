# Servlet에서 alert 창을 띄우고 싶을 때

Servlet(java code)에서 alert 창을 띄우고 싶을 때 아래 코드를 추가해준다.



```java
response.setContentType("text/html; charset=UTF-8");

String message = "다시 입력해 주세요";

PrintWriter writer = response.getWriter();
writer.println("<script>");
writer.println("alert('" + message + "')");
writer.println("location.href = '../index.jsp'");
writer.println("</script>");
writer.close();

// 혹은 함수를 선언해서 사용한다.
public static void alertMessage(HttpServletResponse response, String message) {
        try {
            PrintWriter writer = response.getWriter();
            writer.println("<script>");
            writer.println("alert('" + message + "')");
            writer.println("location.href = '../index.jsp'");
            writer.println("</script>");

            writer.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
   }
```



### 주의할 점

```java
// 아래 코드를 추가해줘야 한다.
writer.close();	

// 아래 코드를 추가하지 않으면 실행되기 전에 아래에 작성된 forward가 실행되어서, alert 메시지가 뜨지 않는다.
request.getRequestDispatcher("stock.jsp").forward(request, response);
```





