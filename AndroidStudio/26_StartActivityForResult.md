# StartActivityForResult

Activity의 창을 전환하는 방법은 2가지가 있다.

1. StartActivitiy
2. StartActivityForResult

1번의 경우는 단방향성으로 (A→B)로 가는 동작으로 끝난다면
2번은 양방향성(A↔B)으로 B의 정보를 다시 A로 가져올 수 있다.

# 결과
![GIF 2020-12-22 오전 1-09-02](https://user-images.githubusercontent.com/39082893/102796953-5a3c9b00-43f2-11eb-9e35-6664c8e870e8.gif)

# 순서

## 1. SubActivity 생성
<img src = "https://user-images.githubusercontent.com/39082893/102797177-aab3f880-43f2-11eb-96a2-b98dccdf892b.png"  width="400"  height="100%"/>

## 2. layout 작성

**_res.layout.activity_main.xml_**
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tv_comeback"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="값을 가져와 주세요."
        android:textSize="30sp"
       />

    <Button
        android:id="@+id/btn_go"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="GO"/>

</LinearLayout>
```
---
**_res.layout.activity_sub.xml_**
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SubActivity">

    <EditText
        android:id="@+id/et_comeback"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:hint="메인으로 보낼 값을 입력해주세요."/>

    <Button
        android:id="@+id/btn_close"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="종료"/>

</LinearLayout>
```
---
## 3. java 코드 작성

**_MainActivity.java_**
```java
package com.mistive.comebackexample;

import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private TextView tv_comeback;
    private Button btn_go;

    private static final int REQUEST_CODE = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tv_comeback = findViewById(R.id.tv_comeback);
        btn_go = findViewById(R.id.btn_go);

        btn_go.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(getApplicationContext(), SubActivity.class);
                startActivityForResult(intent, REQUEST_CODE);
            }
        });

    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if(resultCode == RESULT_OK){
            Toast.makeText(getApplicationContext(), "수신 성공", Toast.LENGTH_SHORT).show();
        }else{
            Toast.makeText(getApplicationContext(), "수신 실패", Toast.LENGTH_SHORT).show();
        }

        if(requestCode == REQUEST_CODE){
            String resultTxt = data.getStringExtra("comeback");
            tv_comeback.setText(resultTxt);
        }
    }
}
```
### 주요 코드
```java
Intent intent = new Intent(getApplicationContext(), SubActivity.class);
startActivityForResult(intent, REQUEST_CODE);
```
`startActivityForResult(Intent, int);`
intent와 int 타입의 REQUEST_CODE가 들어간다. 이는 오버라이드 메소드 중 `onActivityResult()`를 통해 `REQUEST_CODE`를 가지고 어떤 요청에 대한 응답인지 판단할 수 있다.

---
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if(resultCode == RESULT_OK){
        Toast.makeText(getApplicationContext(), "수신 성공", Toast.LENGTH_SHORT).show();
    }else{
        Toast.makeText(getApplicationContext(), "수신 실패", Toast.LENGTH_SHORT).show();
    }

    if(requestCode == REQUEST_CODE){
        String resultTxt = data.getStringExtra("comeback");
        tv_comeback.setText(resultTxt);
    }
}
```
`onActivityResult()` 메소드이다. MainActivity에서 요청한 intent의 `requestCode`, SubActivity에서의 응답인 `resultCode`, 그리고 `data`. 이렇게 3가지 파라미터를 지닌다.

이 파라미터들을 가지고 위와같이 원하는 작업을 수행할 수 있다.

intent를 통해 데이터 값을 받아올 땐 `getStringExtra("_NAME")
`을 활용한다.


---
**_SubActivity.java_**
```java
package com.mistive.comebackexample;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class SubActivity extends AppCompatActivity {

    private EditText et_comeback;
    private Button btn_close;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sub);

        et_comeback = findViewById(R.id.et_comeback);
        btn_close = findViewById(R.id.btn_close);

        btn_close.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent();
                
                String str = et_comeback.getText().toString();
                intent.putExtra("comeback", str);
                setResult(RESULT_OK, intent);
                finish(); //현재 Activity 종료
            }
        });

    }
}
```

### 주요 코드
```java
@Override
public void onClick(View view) {
    Intent intent = new Intent();
    
    String str = et_comeback.getText().toString();
    intent.putExtra("comeback", str);
    setResult(RESULT_OK, intent);
    finish(); //현재 Activity 종료
}
```
intent에 데이터를 실을 때는 `putExtra("_NAME", String)`을 활용한다.

intent에 데이터를 모두 실은 후에  `setResult(RESULT_OK, INTENT)`를 사용하여 intent를 반환한다.
 
 그 후, `finish()`를 이용해 현재 Activity를 종료해준다.
 
 ---
 
