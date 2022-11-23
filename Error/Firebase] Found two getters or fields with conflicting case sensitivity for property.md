# Android



### error

>  com.google.firebase.database.DatabaseException: Found two getters or fields with conflicting case sensitivity for property.



<br>



### 해결

* Firebase 사용 시에, Model의 getter와 setter명이 달라서 발생한 오류이다.
* getter와 setter명을 동일한 형식으로 맞추니 오류가 해결됨