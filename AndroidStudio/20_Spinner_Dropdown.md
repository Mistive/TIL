# Spinner_Dropdown
![image](https://user-images.githubusercontent.com/39082893/102681578-d9b94700-4205-11eb-9bfd-1b583c855e01.png)
## Spinner란?
* 개발자가 만들어놓은 list값 중 하나를 사용자가 선택할 수 있게 하는 것이다.


## 순서
### 1. List 생성
* 드롭다운 메뉴에 들어갈 `array.xml`항목을 만든다.

#### res.values. array.xml
```java
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <string-array name="test">  
        <item>사과</item>  
        <item>배</item>  
        <item>파인애플</item>  
        <item>수박</item>  
        <item>용과</item>  
    </string-array>  
</resources>
```
### 2. UI 설정
* `activity_main.xml `에 spinner를 추가한다.

####  activity_main.xml
```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

   <Spinner
       android:id="@+id/spinner"
       android:layout_width="150dp"
       android:layout_height="40dp"
       android:entries="@array/test">
   </Spinner>
    <TextView
        android:id="@+id/tv_result"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="드롭다운 결과"/>
</LinearLayout>
```
`android:entries="@array/test"` 
: values에 추가한 list를 연결시켜준다.

### 3. java 파일 설정

####  MainActivity.java
```java
package com.mistive.spnnerexample;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.Spinner;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    private Spinner spinner;
    private TextView tv_result;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        spinner = findViewById(R.id.spinner);
        tv_result = findViewById(R.id.tv_result);

        spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
            @Override
            public void onItemSelected(AdapterView<?> adapterView, View view, int i, long l) {
                tv_result.setText(adapterView.getItemAtPosition(i).toString());
            }

            @Override
            public void onNothingSelected(AdapterView<?> adapterView) {

            }
        });
    }
}
```
`spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {...});`
: spinner에서 ltem 선택시 이벤트 처리를 진행하고 싶을 때 사용한다.

<br></br>  

`adapterView.getItemAtPosition(i).toString()`
: adapterView의 item 값을 가져오는 함수이다.


## 결과
![image](https://user-images.githubusercontent.com/39082893/102683931-bd260a80-4217-11eb-9a23-789947fa287f.png)


## 공부
### AdapterView
일정한 디자인 패턴을 가지고 있는 항목을 나열하는 뷰를 의미한다.






