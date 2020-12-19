# Android Studio - 19_ FCM_PushAlarm

#### FCM이란?
`구글의 Firebase 클라우드 메시징(FCM)은 무료로 메시지를 안정적으로 전송할 수 있는 교차 플랫폼 메시징 솔루션`

## 준비
### FCM 사용 조건

-   API 수준 16(Jelly Bean) 이상 타겟팅
-   Gradle 4.1 이상 사용
-   다음 버전 요구사항을 충족하는  [Jetpack(AndroidX)](https://developer.android.com/jetpack/androidx/migrate?hl=ko)  사용
    -   `com.android.tools.build:gradle`  v3.2.1 이상
    -   `compileSdkVersion`  28 이상

##  방법
### 1. Firebase 프로젝트 추가
* 프로젝트 명은 Android 프로젝트의 패키지명과 일치해야 한다.

### 2.   gradle 추가


#### build.gradle(Project)
##### 추가
```java
classpath 'com.google.gms:google-services:4.3.4' 
```
##### 전체
```java
buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.0.2"
        classpath 'com.google.gms:google-services:4.3.4'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```
---
#### build.gradle(Module)
##### 추가
```java
    implementation platform('com.google.firebase:firebase-bom:26.1.1')
    implementation 'com.google.firebase:firebase-analytics'
    implementation 'com.google.firebase:firebase-messaging:20.3.0'
```
##### 전체
```java
dependencies {  
  implementation fileTree(dir: "libs", include: ["*.jar"])  
    implementation 'androidx.appcompat:appcompat:1.2.0'  
  implementation 'androidx.constraintlayout:constraintlayout:2.0.4'  
  testImplementation 'junit:junit:4.12'  
  androidTestImplementation 'androidx.test.ext:junit:1.1.2'  
  androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'  
  implementation platform('com.google.firebase:firebase-bom:26.1.1')  
    implementation 'com.google.firebase:firebase-analytics'  
  implementation 'com.google.firebase:firebase-messaging:20.3.0'  
}
```


### 3. google-services.json 추가  
##### 파일 위치
![image](https://user-images.githubusercontent.com/39082893/102533362-acc34200-40e8-11eb-9596-fd57aa323e99.png)

* 해당 json 파일은 Firebase에서 프로젝트를 생성하면 다운로드 받을 수 있다.

### 4. FirebaseInstanceIDService.java 파일 추가
##### FirebaseInstanceIDService.java
```java
package com.mistive.pushtest;

import android.util.Log;

import com.google.firebase.iid.FirebaseInstanceId;
import com.google.firebase.iid.FirebaseInstanceIdService;

public class FirebaseInstanceIDService extends FirebaseInstanceIdService {

    private static final String TAG = "MyFirebaseIIDService";

    @Override //각 핸드폰 기기에서 받을 수 있는 난수 값. 해당 토큰을 가지고 있는 기기에 메시지 전송 가능
    public void onTokenRefresh() {
        String token = FirebaseInstanceId.getInstance().getToken();
        Log.e(TAG, token);

        sendRegistrationToServer(token);
    }
    private void sendRegistrationToServer(String token) {
    }
}
```
---

### 5. FirebaseMessagingService.java 추가
##### FirebaseMessagingService.java
```java
package com.mistive.pushtest;

import android.app.NotificationManager;
import android.app.PendingIntent;
import android.content.Context;
import android.content.Intent;
import android.media.RingtoneManager;
import android.util.Log;

import androidx.core.app.NotificationCompat;

import com.google.firebase.messaging.RemoteMessage;

public class FirebaseMessagingService extends com.google.firebase.messaging.FirebaseMessagingService{


    private static final String TAG = "FirebaseMsgService";

    private String msg, title;


    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        Log.e(TAG, "onMessageReceived");

        title = remoteMessage.getNotification().getTitle();
        msg = remoteMessage.getNotification().getBody();

        Intent intent = new Intent(this, MainActivity.class);   //new Intent(현재 Activity, 넘어갈 Activity)
        intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);


        //PendingIntent?
        PendingIntent contentIntent = PendingIntent
                .getActivity(this, 0, new Intent(this, MainActivity.class), 0);

        //Push Alarm이 떴을 때의 Icon 설정 + Push 알림이 발생했을 때의 설정
        NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this).setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle(title)
                .setContentText(msg)
                .setAutoCancel(true)
                .setSound(RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION))
                .setVibrate(new long[]{1,1000}); //1000ms. 1초동안 진동

        NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
        notificationManager.notify(0, mBuilder.build());
        mBuilder.setContentIntent(contentIntent);


    }
}
```
------------------------------------------------------------------------





## 이슈

*  앱이 Background에서 실행될 때 알림이 온다.

