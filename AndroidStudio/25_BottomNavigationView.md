# BottomNavigationView

<p align="center"><img src="https://user-images.githubusercontent.com/39082893/102714549-9d274180-4312-11eb-9078-8fd1b318c580.png">
</p>

위 사진과 같이 Bottom Navigation Bar를 만들어보자.

>  참고 : https://developer.android.com/reference/android/support/design/widget/BottomNavigationView
>  https://material.io/design/components/bottom-navigation.html
## 순서

### 01.  bottom_navigation_menu 추가
```xml
<!--res.menu.bottom_navigation_menu.xml-->
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/action_call"
        android:title="@string/calls"
        android:icon="@mipmap/call"/>
    <item
        android:id="@+id/action_chat"
        android:title="@string/chats"
        android:icon="@mipmap/chat"/>
    <item
        android:id="@+id/action_contacts"
        android:title="@string/contacts"
        android:icon="@mipmap/contacts"/>
</menu>
```
* 현재 나는 3개의 item을 추가한 상태이다.
* icon은 따로 다운받아 넣어도 되고, `vector-assert`를 활용해도 된다.

<p align="center"><img src="https://user-images.githubusercontent.com/39082893/102714756-24c18000-4314-11eb-9918-b7da85f7e2d2.png">
</p>
menu directory를 만들고 menu 타입의 리소스를 생성한 후 위와같은 형식으로 아이템을 추가해주면 된다.

###  02. BottomNavigationView 추가
```xml
<!--res.layout.activity_main.xml-->
<com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/bottomNavigationView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@color/colorPrimary"
        app:itemIconTint="@color/bottom_navigation_colors"
        app:itemTextColor="@color/bottom_navigation_colors"
        app:layout_constraintBottom_toBottomOf="parent"
        app:menu="@menu/bottom_navigation_menu"
        tools:layout_editor_absoluteX="2dp" />
```
여기서 봐야할 부분은 

`app:menu="@menu/bottom_navigation_menu"`
아까 만든 menu를 BottomNavigationView와 연결해준다.

 `app:itemIconTint="@color/bottom_navigation_colors"`
 `app:itemTextColor="@color/bottom_navigation_colors"`
 아이콘 색깔과 text색깔을 결정해준다.
 현재 선택된 버튼과 선택되지 않은 버튼 색깔을 selector를 이용하여 다르게 설정해놓았다.
![image](https://user-images.githubusercontent.com/39082893/102714870-fb552400-4314-11eb-9fe0-10ed8b957a77.png)
```xml
<!--res.color.bottom_navigation_colors.xml-->
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:state_checked="true"
        android:color="@color/colorAccent" />
    <item
        android:state_checked="false"
        android:color="#ffffff" />
</selector>
```

### 03. FragmentLayout 추가
```xml
<!--res.layout.activity_main.xml-->
    <FrameLayout
        android:id="@+id/frame"
        android:layout_width="match_parent"
        android:layout_height="673dp"
        app:layout_constraintBottom_toTopOf="@+id/bottomNavigationView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

    </FrameLayout>
```
버튼을 눌렀을 때 fragment가 바뀔 수 있도록 frameLayout을 넣어준다.


### 04. Fragment 추가
버튼이 3개이므로 Fragment를 3개 만들어준다.

#### 04_1. fragment1.xml 생성
```xml
<!--res.layout.fragment1.xml-->

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android" android:layout_width="match_parent"
    android:layout_height="match_parent"

    android:orientation="horizontal"
    android:layout_gravity="center">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="1번 Fragment"
        android:textSize="50sp"
        android:gravity="center"/>

</LinearLayout>
```
![image](https://user-images.githubusercontent.com/39082893/102714947-76b6d580-4315-11eb-9c14-d07756566a8c.png)

#### 04_2. fragment1.java 생성
```java
package com.mistive.bottomnavi;

import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;

public class fragment1 extends Fragment {
    @Nullable
    @Override
    public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment1, container, false);
    }
}
```
Fragment를 상속받고 onCreateView에서 inflater를 이용하여 layout에 있는 fragment와 연결시켜준다.

위와 같은 Fragment를 2개 더 만들어준다.

### 05. MainActivity.java 수정
마지막으로 BottomNavigationBar에서 버튼을 눌렀을 때 fragment가 변하게 설정을 해준다.
```java
package com.mistive.bottomnavi;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.FragmentManager;
import androidx.fragment.app.FragmentTransaction;

import android.os.Bundle;
import android.util.Log;
import android.view.MenuItem;

import com.google.android.material.bottomnavigation.BottomNavigationView;

public class MainActivity extends AppCompatActivity {

    private BottomNavigationView bottomNavigationView;
    private FragmentTransaction ft;
    private fragment1 fg1 = new fragment1();
    private fragment2 fg2 = new fragment2();
    private fragment3 fg3 = new fragment3();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        bottomNavigationView = findViewById(R.id.bottomNavigationView);
        bottomNavigationView.setOnNavigationItemSelectedListener(new BottomNavigationView.OnNavigationItemSelectedListener() {

            @Override
            public boolean onNavigationItemSelected(@NonNull MenuItem menuItem) {
                ft = getSupportFragmentManager().beginTransaction();
                switch(menuItem.getItemId()){
                    case R.id.action_call:
                        ft.replace(R.id.frame, fg1).commit();
                        return true;
                    case R.id.action_chat:
                        ft.replace(R.id.frame, fg2).commit();
                        return true;
                    case R.id.action_contacts:
                        ft.replace(R.id.frame, fg3).commit();
                        return true;
                }
                return true;
            }
        });
        ft = getSupportFragmentManager().beginTransaction();
        ft.replace(R.id.frame, fg1).commit();
    }


}
```
### ※ fragment 변환 시 주의 사항
``` 
ft = getSupportFragmentManager().beginTransaction();
ft.replace(R.id.frame, fg1).commit();  
```
`beginTransaction()`이 `commit()` 이전에 선행되어야 함.

## 결과
![GIF 2020-12-20 오후 11-09-16](https://user-images.githubusercontent.com/39082893/102715384-6eac6500-4318-11eb-8156-c6abbc698e7b.gif)

  
