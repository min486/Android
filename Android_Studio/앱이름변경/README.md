<div align="center">
  <p>
    <img src="../README.assets/studio.png">
  </p>
  <br>
  <h2>Android Studio</h2>
  <p>안드로이드 스튜디오 관련 내용 정리</p>
  <br>
  <br>
</div>


## 🔥 안드로이드 앱 이름 변경

### 앱 이름 변경 방법

앱 시작 전 런처(홈 화면) 에서 보이는 앱 이름을 변경하려면

`AndroidManifest.xml` 파일에서 `<application>` 태그 내의 `android:label` 속성을 수정하면 된다

```xml
<application
    android:icon="@mipmap/ic_launcher"
    android:label="새로운 앱 이름"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/Theme.MyApplication">
</application>
```

👉 이렇게 변경하면 앱 설치 후 홈 화면에 "새로운 앱 이름" 으로 표시된다

<br>

### 앱 이름이 변경되지 않는 경우

만약 `<application>` 태그에 `label` 속성이 있음에도 앱 이름이 바뀌지 않는다면,

 `<activity>` 태그에 별도의 `label` 속성이 지정되어 있을 수 있다

<br>

`<activity>`의 `label` 값은 `<application>`의 `label` 값을 덮어쓰기 때문에,

최종적으로 홈 화면에는 `<activity>`에 지정한 이름이 표시된다

```xml
<application
    android:label="@string/app_name"  ...>
    <activity
        android:name=".MainActivity"
        android:label="다른 이름"  ...>
        ...
    </activity>
</application>
```

👉 이 경우 `<activity>` 태그의 `label` 속성을 제거하면 `<application>` 이름이 적용된다
