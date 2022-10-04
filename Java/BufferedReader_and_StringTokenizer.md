> reference link: [GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-scanner-and-bufferreader-class-in-java/), 
[personal blog-1](https://www.java67.com/2016/06/5-difference-between-bufferedreader-and-scanner-in-java.html), 
[personal blog-2](https://javahungry.blogspot.com/2018/12/difference-between-bufferedreader-and-scanner-in-java-examples.html)

# Java에서 문자열 입력받기
In Java, Scanner and BufferedReader class are sources that serve as ways of reading inputs.



<br>



## 1. BufferedReader vs Scanner in Java

1. **Buffer Memory:** BufferedReader has larger buffer memory(8KB or 8192 chars) than Scanner which has (1KB or 1024 chars). It means if you are reading a large String than you should use BufferedReader. If the user input is short or other than String, you can use Scanner.

2. **Functionality:** BufferedReader is used to read the data only. Scanner not only read the data but also parse the data.

Scanner uses regular expression to read and parse text. 
It can use custom delimiter and parse text into primitive data type for e,g short, long, int 
using `nextShort(), nextLong(), nextInt()` methods. BufferedReader can only read String using readLine() method.

3. **Performance:** BufferedReader is faster than Scanner because BufferedReader does not need to parse the data.

4. **Data type:** BufferedReader can read only String. Scanner can read String as well as primitive data types (int, float, double, long, short).

5. **Introduction to JDK:** BufferedReader was introduced in JDK 1.1 while Scanner was introduced in JDK1.5 .

6. **Synchronization:** BufferedReader is synchronized while Scanner is not. For multi-threading applications BufferedReader is preferred.

7. **Checked Exception:** BufferedReader throws CheckedException(i.e IOException) while Scanner does not throw any CheckedException.

8. **Package:** BufferedReader class is present in java.io package while Scanner class is present in java.util package.

9. Scanner는 Space, Enter 모두 경계로 입력값을 인식하는 반면, BufferedReader는 Enter만을 경계로 입력값을 인식한다.



<br>



## 2. Example



### 2-1. BufferedReader
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();    
        sb.append(bufferedReader.readLine());
        
        // default type is String. So, type conversion is needed.
        int num = Integer.parseInt(bufferedReader.readLine());
        
        bufferedReader.close();
   }
}
```

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String str = br.readLine();

        // blank separate method 1
        StringTokenizer st = new StringTokenizer(str);
        int a = Integer.parseInt(st.nextToken());
        int b = Integer.parseInt(st.nextToken());

        // blank separate method 2
        String [] strArr = str.split(" ");

        br.close();
    }
}
```



### 2-2. StringTokenizer

* BufferedReader의 단점은 개행(\n)밖에 구분하지 못하는 것이다. 입력값으로 "Hello World"가 주어지면 buffer는 
Hello와 World를 구분하지 못하는데, 이를 도와주는 것이 StringTokenzier이다. 
* StringTokenizer st = new StringTokenizer(br.readLine(), "구분자 혹은 공백");
* 구분자 여러개 등록: new StringTokenizer(br.readLine(), ",|+");  `|` 사용

```java
// 백준 4344, 여러 line을 입력받는 경우 예시 코드

public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine()); // 공백 단위로 읽어들일 수 있는 라인 추가
        
        int C = Integer.parseInt(st.nextToken());

        // test case
        for (int i = 0; i < C; i++) {
            st = new StringTokenizer(br.readLine());
            ArrayList<Integer> Arr = new ArrayList<>();
            // 학생 수
            int stuCnt = Integer.parseInt(st.nextToken());

            // 학생 수만큼 스코어 입력
            for(int j =0; j<stuCnt; j++) {
                int score = Integer.parseInt(st.nextToken());
                Arr.add(score);
            }

        }
```



#### 2-2-1. StringTokenizer 2번 사용

```java
// 백준 1026. StringTokenizer를 두 번 생성하는 경우
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;
import java.util.StringTokenizer;

public class Main {
    // 1026
    public static void main(String[] args) throws IOException {

        Main main = new Main();
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        int N = Integer.parseInt(br.readLine());
        int [] arr = new int[N];
        int [] brr = new int[N];

        st = new StringTokenizer(br.readLine(), " ");
        for (int i = 0; i < N; i++) {
          arr[i] = Integer.parseInt(st.nextToken());
        }
        st = new StringTokenizer(br.readLine(), " ");
        for (int j = 0; j < N; j++) {
            brr[j] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(arr);
        Arrays.sort(brr);
        int ans = 0;

        for (int i = 0; i < N; i++) {
            ans += arr[i] * brr[N - 1- i];
        }
        System.out.println(ans);

        br.close();
    }
}
```

-> 여기서 왜 for문을 반복할 때 마다 StringTokenizer를 새로 생성해야 하는지 처음에는 이해가 되지 않았으나, [ tistory blog](https://kephilab.tistory.com/99) 해당 링크를 참고해서 대략적으로 이해한 것 같다.

내가 이해한 것은, `StringTokenizer st = new StringTokenizer(br.readLine(), " ");` 와 같이 StringTokenizer 객체를 생성하면, br.readLine() 로 받아오는 문자열을 `st.nextToken()` 을 사용하여 하나씩 토큰을 꺼내온다. 따라서, 받아오는 문자열이 끝나면, 즉 st 객체에서 더 이상 가져올 토큰이 없다면, nextToken() 메소드는 java.util.NoSuchElementException 예외를 발생시킨다.

따라서, 반복문에서 문자열을 받아올 때 마다, StringTokenizer 객체를 생성해야 한다.
