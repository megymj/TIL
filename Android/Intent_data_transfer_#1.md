## Intent로 데이터를 다음 Activity로 전달하여 ImageView 객체를 동적으로 생성하기

모바일프로그래밍 프로젝트 Triplanner 내용 中

화면: PlacePlanner.java -> Day1PlacePlanner.java 로 페이지 이동하도록 설정

<br>



### 상황

* 아래는 planner_place.xml 코드 중 일부
* 현재 RelativeLayout 안에 ImageView, TextView, Button이 포함되어 있는데, RelativeLayout 전체 혹은 ImageView, TextView, Button 각각을 Intent 객체 혹은 기타 다른 방법을 사용하여, 다음 Activity 화면으로 데이터를 전달하여 화면에 띄우는 것이 목표이다.

```xml
...

<LinearLayout
        
     <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="30dp"
        android:gravity="center" >

        <!--목록-->
        <RelativeLayout
            android:id="@+id/cafe1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:visibility="gone">
            <ImageView
                android:id="@+id/cafeView1"
                android:scaleType="centerCrop"
                android:layout_width="match_parent"
                android:layout_height="120dp"
                android:alpha="0.7"
                android:src="@drawable/img_planner_place_cafe_1"/>
            <TextView
                android:id="@+id/cafeText1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerVertical="true"
                android:layout_margin="20dp"
                android:text="카페 이름"
                android:textSize="11pt"
                android:textColor="@android:color/white"/>
            <Button
                android:id="@+id/cafeTag1"
                android:layout_width="60dp"
                android:layout_height="24dp"
                android:layout_alignEnd="@id/cafeView1"
                android:layout_centerVertical="true"
                android:layout_margin="20dp"
                android:clickable="false"
                android:background="@drawable/main_post_postplacetag_tagstyle"
                android:backgroundTint="#6CE4B4"
                android:text="카 페"
                android:textSize="6pt"
                android:textAlignment="center"
                android:textColor="@android:color/white"/>
        </RelativeLayout>

        <RelativeLayout
            android:id="@+id/att1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:visibility="gone">
            <ImageView
                android:id="@+id/attView1"
                android:scaleType="centerCrop"
                android:layout_width="match_parent"
                android:layout_height="120dp"
                android:alpha="0.7"
                android:src="@drawable/img_planner_place_attraction_1"/>
            <TextView
                android:id="@+id/attText1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerVertical="true"
                android:layout_margin="20dp"
                android:text="명소 이름"
                android:textSize="11pt"
                android:textColor="@android:color/white"/>
            <Button
                android:id="@+id/attTag1"
                android:layout_width="60dp"
                android:layout_height="24dp"
                android:layout_alignEnd="@id/attView1"
                android:layout_centerVertical="true"
                android:layout_margin="20dp"
                android:clickable="false"
                android:background="@drawable/main_post_postplacetag_tagstyle"
                android:backgroundTint="#FFD37A"
                android:text="명 소"
                android:textSize="6pt"
                android:textAlignment="center"
                android:textColor="@android:color/white"/>
        </RelativeLayout>

        <RelativeLayout
            android:id="@+id/rest1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:visibility="gone">
            <ImageView
                android:id="@+id/restView1"
                android:scaleType="centerCrop"
                android:layout_width="match_parent"
                android:layout_height="120dp"
                android:alpha="0.7"
                android:src="@drawable/img_planner_place_restaurant_1"/>
            <TextView
                android:id="@+id/restText1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_centerVertical="true"
                android:layout_margin="20dp"
                android:text="맛집 이름"
                android:textSize="11pt"
                android:textColor="@android:color/white"/>
            <Button
                android:id="@+id/restTag1"
                android:layout_width="60dp"
                android:layout_height="24dp"
                android:layout_alignEnd="@id/restView1"
                android:layout_centerVertical="true"
                android:layout_margin="20dp"
                android:clickable="false"
                android:background="@drawable/main_post_postplacetag_tagstyle"
                android:backgroundTint="#F26C73"
                android:text="맛 집"
                android:textSize="6pt"
                android:textAlignment="center"
                android:textColor="@android:color/white"/>
        </RelativeLayout>

    </LinearLayout>
</LinearLayout>
...
```



<br>

### 시도 1. Intent 객체를 사용해서 다음 Activity에서 ImageView를 동적으로 생성되도록 

* PlacePlanner.java (위의 planner_place.xml 코드에 대응되는 .java 파일)

```java
package com.seoultech.triplanner;

import android.content.Intent;
import android.content.res.ColorStateList;
import android.graphics.Color;
import android.os.Bundle;
import android.os.Parcelable;
import android.view.View;
import android.widget.Button;
import android.widget.ImageButton;
import android.widget.RelativeLayout;

import androidx.appcompat.app.AppCompatActivity;

public class PlacePlanner extends AppCompatActivity {

    RelativeLayout att1, rest1, cafe1;
    Button btnNext;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_place);

        att1 = (RelativeLayout) findViewById(R.id.att1);
        rest1 = (RelativeLayout) findViewById(R.id.rest1);
        cafe1 = (RelativeLayout) findViewById(R.id.cafe1);

        btnNext = (Button) findViewById(R.id.btnNext);

        // RelativeLayout 을 클릭 시 이미지들을 저장하도록 하는 Intent 객체를 생성
        // 여기서 final keyword 를 사용하지 않으면, 아래에서 오류 발생
        final Intent intent = new Intent(PlacePlanner.this, Day1PlacePlanner.class);
				
        
      	// ... 중간 코드 생략
      
      
        // 여기서, att1, rest1, cafe1 의 RelativeLayout 을 클릭할 때, intent 로 data 를 저장한다.
        att1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                intent.putExtra("att1Img", R.drawable.img_planner_place_attraction_1);
            }
        });
        rest1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                intent.putExtra("rest1Img", R.drawable.img_planner_place_restaurant_1);
            }
        });
        cafe1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
               intent.putExtra("cafe1Img", R.drawable.img_planner_place_cafe_1);
            }
        });

        
        // 클릭 시 다음 화면으로 이동
       btnNext.setOnClickListener(new View.OnClickListener() {
           @Override
           public void onClick(View view) {
               startActivity(intent);
           }
       });   
    }
}
```



<br>



* Day1PlacePlanner.java: PlannerPlace.java에서 btnNext button을 클릭 시 이동하는 화면

  * 참고 링크: [How to Send Image File from One Activity to Another Activity?, geeksforgeeks](https://www.geeksforgeeks.org/how-to-send-image-file-from-one-activity-to-another-activity/)

  * RelativeLayout을 Intent 혹은 Bundle 객체로 보내는 법을 찾지 못해서, 우선 ImageView만을 동적으로 생성하도록 하였고, 이후 TextView나 Button도 동적으로 생성하도록 코드를 작성하도록 하면 된다(여기서는 구현하지 않음). 하지만 java코드에서 동적으로 생성하기 때문에, TextView나 Button의 배치 조절이 조금 어려울 것으로 예상된다.

```java
package com.seoultech.triplanner;

import android.graphics.Color;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.RelativeLayout;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

import java.util.LinkedHashSet;
import java.util.Set;

public class Day1PlacePlanner extends AppCompatActivity {

    int imageValue;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_place_day1);

        final Bundle bundle = getIntent().getExtras();
        //System.out.println(bundle.size());  // bundle의 사이즈에 맞도록 imageView를 생성하자.

        if (bundle != null) {
            Set<String> keySet = bundle.keySet();   // intent 객체로 받아온 전체 keySet

          // 여기서 LinearLayout의 id를 가져와서 생성한다
            LinearLayout linearLayout = (LinearLayout) findViewById(R.id.imgLinearLayout);

            for (String s : keySet) {
                // s값: att1Img, cafe1Img, rest1Img
                ImageView imgView = new ImageView(this);
                imageValue = bundle.getInt(s);
                imgView.setImageResource(imageValue);
                imgView.setScaleType(ImageView.ScaleType.CENTER_CROP);	// 이 코드를 사용해야 drawable이 화면에 딱 맞게 크기가 설정된다.

                LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT,300);
                layoutParams.setMargins(20, 20, 20, 20);
                imgView.setLayoutParams(layoutParams);
                linearLayout.addView(imgView);
              
            }
        }
    }
}
```



* planner_place_day1.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/black"
    android:weightSum="100"
    android:orientation="vertical"
    android:id="@+id/topLinear">
  
   <!-- 코드 생략 -->

    <!--Main-->
    <LinearLayout
        android:id="@+id/MainWindow"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="84"
        android:orientation="vertical">
        <TextView
            android:id="@+id/textView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:layout_marginTop="30dp"
            android:layout_marginBottom="20dp"
            android:textSize="24dp"
            android:textColor="@android:color/white"
            android:text="1일차 활동 선택 내역"/>

        <ScrollView
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <LinearLayout
                android:id="@+id/imgLinearLayout"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:orientation="vertical">
              
						<!-- 여기 위치에 Day1PlacePlanner.java 파일에서 ImageView가 동적으로 생성되도록 -->
           

        </ScrollView>
          
    </LinearLayout>
</LinearLayout>
```



<br>

### 시도 2. xml 파일을 include하는 방식으로 사용

* places.xml
  * planner_place.xml 파일에서 RelativeLayout 부분 코드를 분리함

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_weight="84"
    android:orientation="vertical">

    <!--목록: planner_place_*.xml 에서 사용하는 장소의 목록들을
    이 부분에 작성한다
     이 파일을 다른 .xml 파일에서 사용할 때는, include 해서 사용-->
    <RelativeLayout
        android:id="@+id/cafe1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:visibility="gone">
        <ImageView
            android:id="@+id/cafeView1"
            android:scaleType="centerCrop"
            android:layout_width="match_parent"
            android:layout_height="120dp"
            android:alpha="0.7"
            android:src="@drawable/img_planner_place_cafe_1"/>
        <TextView
            android:id="@+id/cafeText1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerVertical="true"
            android:layout_margin="20dp"
            android:text="카페 이름"
            android:textSize="11pt"
            android:textColor="@android:color/white"/>
        <Button
            android:id="@+id/cafeTag1"
            android:layout_width="60dp"
            android:layout_height="24dp"
            android:layout_alignEnd="@id/cafeView1"
            android:layout_centerVertical="true"
            android:layout_margin="20dp"
            android:clickable="false"
            android:background="@drawable/main_post_postplacetag_tagstyle"
            android:backgroundTint="#6CE4B4"
            android:text="카 페"
            android:textSize="6pt"
            android:textAlignment="center"
            android:textColor="@android:color/white"/>
    </RelativeLayout>

    <RelativeLayout
        android:id="@+id/att1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:visibility="gone">
        <ImageView
            android:id="@+id/attView1"
            android:scaleType="centerCrop"
            android:layout_width="match_parent"
            android:layout_height="120dp"
            android:alpha="0.7"
            android:src="@drawable/img_planner_place_attraction_1"/>
        <TextView
            android:id="@+id/attText1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerVertical="true"
            android:layout_margin="20dp"
            android:text="명소 이름"
            android:textSize="11pt"
            android:textColor="@android:color/white"/>
        <Button
            android:id="@+id/attTag1"
            android:layout_width="60dp"
            android:layout_height="24dp"
            android:layout_alignEnd="@id/attView1"
            android:layout_centerVertical="true"
            android:layout_margin="20dp"
            android:clickable="false"
            android:background="@drawable/main_post_postplacetag_tagstyle"
            android:backgroundTint="#FFD37A"
            android:text="명 소"
            android:textSize="6pt"
            android:textAlignment="center"
            android:textColor="@android:color/white"/>
    </RelativeLayout>

    <RelativeLayout
        android:id="@+id/rest1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:visibility="gone">
        <ImageView
            android:id="@+id/restView1"
            android:scaleType="centerCrop"
            android:layout_width="match_parent"
            android:layout_height="120dp"
            android:alpha="0.7"
            android:src="@drawable/img_planner_place_restaurant_1"/>
        <TextView
            android:id="@+id/restText1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerVertical="true"
            android:layout_margin="20dp"
            android:text="맛집 이름"
            android:textSize="11pt"
            android:textColor="@android:color/white"/>
        <Button
            android:id="@+id/restTag1"
            android:layout_width="60dp"
            android:layout_height="24dp"
            android:layout_alignEnd="@id/restView1"
            android:layout_centerVertical="true"
            android:layout_margin="20dp"
            android:clickable="false"
            android:background="@drawable/main_post_postplacetag_tagstyle"
            android:backgroundTint="#F26C73"
            android:text="맛 집"
            android:textSize="6pt"
            android:textAlignment="center"
            android:textColor="@android:color/white"/>
    </RelativeLayout>

</LinearLayout>
```



* planner_place.xml, planncer_place_day1.xml
  * places.xml 파일을 include하는 방식으로 사용

```xml
<include layout="@layout/places" />
```

* PlacePlanner.java
  * putExtra("att1", "att1") 에서 두 번째 인자(넘겨주는 값)을 RelativeLayout의 id와 동일한 String 값을 넘겨주도록 하였다.

```java
      // 여기서, att1, rest1, cafe1 의 RelativeLayout 을 클릭할 때, intent 로 data 를 저장한다.
        att1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                intent.putExtra("att1", "att1");
            }
        });
        rest1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                intent.putExtra("rest1", "rest1");
            }
        });
        cafe1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
               intent.putExtra("cafe1Img", "cafe1");
            }
        });

        
        // 클릭 시 다음 화면으로 이동
       btnNext.setOnClickListener(new View.OnClickListener() {
           @Override
           public void onClick(View view) {
               startActivity(intent);
           }
       });   
    }
}
```



* Day1PlacePlanner.java
  * RelativeLayout을 선언한 뒤, switch문을 통해 가져오도록 구현

```java
package com.seoultech.triplanner;

import android.graphics.Color;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.RelativeLayout;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

import java.util.LinkedHashSet;
import java.util.Set;

public class Day1PlacePlanner extends AppCompatActivity {

    RelativeLayout att1, cafe1, rest1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_place_day1);

        final Bundle bundle = getIntent().getExtras();

        att1 = (RelativeLayout) findViewById(R.id.att1);
        cafe1 = (RelativeLayout) findViewById(R.id.cafe1);
        rest1 = (RelativeLayout) findViewById(R.id.rest1);

        if (bundle != null) {
            Set<String> keySet = bundle.keySet();   // intent 객체로 받아온 전체 keySet

            for (String s : keySet) {
  									switch (s) {
                    case "att1":
                        att1.setVisibility(View.VISIBLE);
                        break;
                    case "rest1":
                        rest1.setVisibility(View.VISIBLE);
                        break;
                    case "cafe1":
                        cafe1.setVisibility(View.VISIBLE);
                        break;
                }
            }
        }
    }
}
```



