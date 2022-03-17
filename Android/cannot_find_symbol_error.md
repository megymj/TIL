# cannot find symbol class Button 오류

```java
public class MainActivity extends AppCompatActivity {

    Button btn1;      // 오류 발생
    TextView text1;   // 오류 발생

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    // 버튼을 누르면 동작할 리스너
    class ButtonListener implements View.OnClickListener{

        @Override
        public void onClick(View view) {
            text1.setText("A Button is clicked!");
        }
    }
}
```

**해결방법**
* 매우 단순했다. 그냥 클래스를 import 시켜주면 됨 `import android.widget.Button;`
* 혹은 마우스 커서를 놔둔 뒤, 메시지 문구에서 import 부분 눌러주면 된다.

**자동으로 import 하도록 setting**
* `File > Settings > Editor > General > Auto import`에서,  
![java](https://user-images.githubusercontent.com/80478750/157669526-82861eb9-35d2-4809-81b8-93bbf884e0a7.PNG)  
 Add ... 부분과 Optimize ... 부분을 체크한 뒤, Apply하면 된다.
