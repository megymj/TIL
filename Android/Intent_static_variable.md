# static 변수를 사용해서 Intent 결과 값 저장하기

모바일프로그래밍 프로젝트 Triplanner 내용 中

중간 발표 이전까지의 코드 정리



## 1. 날짜 정보 받아오기

```java
// PlaceIntent.java

public class PlaceIntent {

    // DatePlanner.java 에서 시작 날짜와 종료 날짜를 입력받기 위해 구현
    static Map<String, Integer> savedDateMap = new LinkedHashMap<>();
  
  	// 이하 생략

    PlaceIntent() {

    }
}
```

```java
// DatePlanner.java
/* CalendarView에서 시작 날짜와 종료 날짜가 선택되면, 해당 날짜를 받아오도록 한다. 다만, O월 O일 ~ O월 O일로 구현하기에는 조금 번거로워서, 일단 처음 날짜를 1일차로 기준을 잡고, 1일차, 2일차, ... 이런 식으로 작성되도록 하기 위해 startDay와 endDay에 (startDay - 1)를 뺀 값을 저장했다.
*/
btnNext.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if (calendarView.getSelectedDates().isEmpty()) {
                    Toast.makeText(getApplicationContext(),
                            "여행 기간을 설정해 주세요!", Toast.LENGTH_SHORT).show();
                    textViewSelectedDate.setText("-");
                }
                else {
                    // 여기서 시작 일자와 끝 일자를 저장한다.
                    PlaceIntent.savedDateMap.put("startDay", startDay - (startDay - 1));
                    PlaceIntent.savedDateMap.put("endDay", endDay - (startDay - 1));

                    Intent intent = new Intent(DatePlanner.this, PlacePlanner.class);
                    startActivity(intent);  // Activity 이동
                }
            }
        });
```

```java
// PlacePlanner.java

// 1일차, 2일차, ... 처럼 나오도록 textView 설정
Integer day = PlaceIntent.savedDateMap.get("startDay");
textView.setText(Integer.toString(day) + "일차 활동을 선택하세요");
```

```java
// SelectedPlanner.java
/* 
PlaceIntent.savedDateMap.get("startDay") < PlaceIntent.savedDateMap.get("endDay")) 값을 비교해서 값이 작으면 이전 화면(활동 선택)으로, 값이 크면 종료 화면으로 이동시킴
*/

btnNext.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    int startDay = PlaceIntent.savedDateMap.get("startDay");
                    int endDay = PlaceIntent.savedDateMap.get("endDay");

                    if (startDay < endDay) {

                        // 여기서 PlacePlanner 의 날짜 값을 +1 증가시킴
                        PlaceIntent.savedDateMap.put("startDay", startDay + 1);
                        
                        Intent intentBack = new Intent(SelectedPlanner.this, PlacePlanner.class);
                        startActivity(intentBack);
                    }
                    else {
                        Intent intentFinish = new Intent(SelectedPlanner.this, FinishPlanner.class);
                        startActivity(intentFinish);
                    }
                }
            });
```



<br>



## 2. Intent로 데이터 전달하기

```java
// PlaceIntent.java

public class PlaceIntent {

    static Intent placeIntent = new Intent();

    PlaceIntent() {

    }
}
```

```java
// PlacePlanner.java

public class PlacePlanner extends AppCompatActivity {

    RelativeLayout att1, rest1, cafe1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_place);

        att1 = (RelativeLayout) findViewById(R.id.att1);
        rest1 = (RelativeLayout) findViewById(R.id.rest1);
        cafe1 = (RelativeLayout) findViewById(R.id.cafe1);
      
        // RelativeLayout 을 클릭 시 이미지들을 저장하도록 하는 Intent 객체를 불러온다.
        PlaceIntent.placeIntent = new Intent();
        PlaceIntent.placeIntent.setClass(PlacePlanner.this, SelectedPlanner.class);

        // 여기서, att1, rest1, cafe1 의 RelativeLayout 을 클릭할 때, intent 로 data 를 저장한다.
        // putExtra 에서 첫 번째 변수는 임의로 지정한 문자열, 두 번째 변수는 RelativeLayout 의 id와 동일한 문자열.
        att1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                PlaceIntent.placeIntent.putExtra("att1_key", "att1");
                startActivity(PlaceIntent.placeIntent);
            }
        });
        rest1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                PlaceIntent.placeIntent.putExtra("rest1_key", "rest1");
                startActivity(PlaceIntent.placeIntent);
            }
        });
        cafe1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                PlaceIntent.placeIntent.putExtra("cafe1_key", "cafe1");
                startActivity(PlaceIntent.placeIntent);
            }
        });
    }
}
```

```java
// SelectedPlanner.java

public class SelectedPlanner extends AppCompatActivity {

    RelativeLayout att1, cafe1, rest1;
    Button cafeDelete1, attDelete1, restDelete1;
    ImageButton imgBtnAddPlace;
    Button btnNext;

    Bundle bundle;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_selected);

        att1 = (RelativeLayout) findViewById(R.id.att1);
        cafe1 = (RelativeLayout) findViewById(R.id.cafe1);
        rest1 = (RelativeLayout) findViewById(R.id.rest1);

        btnNext = (Button) findViewById(R.id.btnNext);

        // PlacePlanner.java 에서 putExtra 로 담은 내용을 bundle 에 담는다.
        bundle = getIntent().getExtras();

        if (bundle != null) {
            final Set<String> keySet = bundle.keySet();   // intent 객체로 받아온 전체 keySet

            for (String s : keySet) {
                // s값: att1_key, cafe1_key, rest1_key

                switch (s) {
                    case "att1_key":
                        att1.setVisibility(View.VISIBLE);
                        attDelete1.setVisibility(View.VISIBLE);
                        break;
                    case "rest1_key":
                        rest1.setVisibility(View.VISIBLE);
                        restDelete1.setVisibility(View.VISIBLE);
                        break;
                    case "cafe1_key":
                        cafe1.setVisibility(View.VISIBLE);
                        cafeDelete1.setVisibility(View.VISIBLE);
                        break;
                }
            }
        }
    }
}
```



<br>

## 3. removedPlaceList로 일차 마다 담긴 부분 초기화하기

```java
// PlaceIntent.java

public class PlaceIntent {

    // 1일차, 2일차마다 활동 담긴 부분을 초기화해주기 위해 사용
    static Map<String, Boolean> removedPlaceList;

    PlaceIntent() {

    }
}

```

```java
// SelectedPlanner.java

public class SelectedPlanner extends AppCompatActivity {

    RelativeLayout att1, cafe1, rest1;
    Button cafeDelete1, attDelete1, restDelete1;
    ImageButton imgBtnAddPlace;

    Bundle bundle;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_selected);

        att1 = (RelativeLayout) findViewById(R.id.att1);
        cafe1 = (RelativeLayout) findViewById(R.id.cafe1);
        rest1 = (RelativeLayout) findViewById(R.id.rest1);

        cafeDelete1 = (Button) findViewById(R.id.cafeDelete1);
        attDelete1 = (Button) findViewById(R.id.attDelete1);
        restDelete1 =(Button) findViewById(R.id.restDelete1);

        imgBtnAddPlace = (ImageButton) findViewById(R.id.imgBtnAddPlace);
        btnNext = (Button) findViewById(R.id.btnNext);

        // PlacePlanner.java 에서 putExtra 로 담은 내용을 bundle 에 담는다.
        bundle = getIntent().getExtras();
        System.out.println("bundle size is : " + bundle.size());

      
        // 여기서 PlaceIntent.removedPlaceList를 생성한다.
        PlaceIntent.removedPlaceList = new LinkedHashMap<>();

        if (bundle != null) {
            final Set<String> keySet = bundle.keySet();   // intent 객체로 받아온 전체 keySet

          	// 일단 받아온 값을 모두 true로 설정한다.
            for (String s : keySet) {
                PlaceIntent.removedPlaceList.put(s, true);
            }

            for (String s : keySet) {
                // s값: att1_key, cafe1_key, rest1_key

                switch (s) {
                    case "att1_key":
                        att1.setVisibility(View.VISIBLE);
                        attDelete1.setVisibility(View.VISIBLE);
                        break;
                    case "rest1_key":
                        rest1.setVisibility(View.VISIBLE);
                        restDelete1.setVisibility(View.VISIBLE);
                        break;
                    case "cafe1_key":
                        cafe1.setVisibility(View.VISIBLE);
                        cafeDelete1.setVisibility(View.VISIBLE);
                        break;
                }
            }

            // Delete button 을 누르면 data 제거
            cafeDelete1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                  // 선택되면 화면을 View.GONE 으로 설정
                   cafe1.setVisibility(View.GONE);

                   // 여기서 bundle 객체의 데이터를 제거한다. false값을 지정.
                    PlaceIntent.removedPlaceList.put("cafe1_key", false);
                }
            });
            attDelete1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    att1.setVisibility(View.GONE);

                    // 여기서 bundle 객체의 데이터를 제거한다. false값을 지정.
                    PlaceIntent.removedPlaceList.put("att1_key", false);
                }
            });
            restDelete1.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    rest1.setVisibility(View.GONE);

                    // 여기서 bundle 객체의 데이터를 제거한다. false값을 지정.
                    PlaceIntent.removedPlaceList.put("rest1_key", false);
                }
            });

        }
    }
}

```

```java
// PlacePlanner.java

public class PlacePlanner extends AppCompatActivity {

    ImageButton imgBtnBack;

    TextView textView;
    RelativeLayout att1, rest1, cafe1;
    Button btnAttraction, btnRestaurant, btnCafe;
    Button btnNext;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.planner_place);

        textView = (TextView) findViewById(R.id.textView);
        btnAttraction = (Button) findViewById(R.id.btnAtt);
        btnRestaurant = (Button) findViewById(R.id.btnRes);
        btnCafe = (Button) findViewById(R.id.btnCafe);

        att1 = (RelativeLayout) findViewById(R.id.att1);
        rest1 = (RelativeLayout) findViewById(R.id.rest1);
        cafe1 = (RelativeLayout) findViewById(R.id.cafe1);

        btnNext = (Button) findViewById(R.id.btnNext);

        
        Set<String> removedListKeySet = null;	// null로 초기화
        if (PlaceIntent.removedPlaceList != null) {	
            removedListKeySet = PlaceIntent.removedPlaceList.keySet();
        }

        if (removedListKeySet != null) {
            for (String s : removedListKeySet) {
                if (!PlaceIntent.removedPlaceList.get(s)) { // 값이 false이면 Intent에서 제거한다. 즉, SelectedPlanner.java 화면에서 해당 내역이 뜨지 않도록 방지한다.
                    PlaceIntent.placeIntent.removeExtra(s);
                }
            }
        }

    }

}
```



<br>

## 4. savedPlacesMap으로 선택했던 장소와 일자 담기

```java
// PlaceIntent.java

public class PlaceIntent {

    // 1일차에 해당하는 장소를 담는 Map(= Python의 Dict)
    // Key 값은 중복이 될 수 없으므로, 장소를 String[] 배열로 받도록 선언(장소를 여러 개 선택 할 수도 있으므로)
    static Map<Integer, LinkedList<String>> savedPlacesMap = new LinkedHashMap<>();

    PlaceIntent() {

    }
}
```

```java
// SelectedPlanner.java

 btnNext.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View view) {
                    int startDay = PlaceIntent.savedDateMap.get("startDay");
                    int endDay = PlaceIntent.savedDateMap.get("endDay");

                    // 각 일차마다 저장된 값을 Place.savedPlacesMap 에 넣어준다.
                    LinkedList<String> list = new LinkedList<>();
                    for (String s : keySet) {
                        list.add(s);
                    }
                    PlaceIntent.savedPlacesMap.put(startDay, list);
                  
```











