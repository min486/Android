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


## 🔥 앱 버전

### 버전 설정

버전 설정에는 2가지가 있다

👉 versionCode, versionName의 값을 모두 정의한다

<br>

### versionCode

- 양의 정수이며, 내부 버전 번호로 사용된다
- 이 번호는 한 버전이 다른 버전보다 최신인지를 판단하는 데 도움이 되며, 번호가 높을수록 최신 버전임을 나타낸다
- 사용자에게 표시되는 버전 번호가 아니다

<br>

### versionName

- 사용자에게 표시되는 버전 번호로 사용된다
- 사용자에게 표시되는 유일한 값이다

<br>

### 버전 값 정의

app 수준의 build.gradle에서 수정 가능하다

```kotlin
android {
    ...
    defaultConfig {
        ...
        versionCode 2
        versionName "1.1"
    }
    ...
}
    
```

<br>

### API 수준 요구사항 지정

앱이 Android 플랫폼의 특정 최소 버전을 요구하는 경우

app 수준의 build.gradle 파일에서 이러한 버전 요구사항을 지정할 수 있다

API 수준 요구사항을 지정하면 호환되는 Android 플랫폼 버전을 실행하는 기기에만 앱이 설치된다
